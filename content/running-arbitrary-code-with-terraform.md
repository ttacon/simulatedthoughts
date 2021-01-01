+++
title = "Running arbitrary code with terraform"
date = 2020-07-23
+++

# Running arbitrary code with terraform
I've recently been spending a lot of time thinking about zero trust systems
and how to merge that ethos with the developer experience. This has lead me down
a deep, deep rabbit whole trying to identify a whole slew of ways to get a foothold
on developer systems and how to move laterally once there. All in all, the modern
developer ecosystem is _highly_ trusting, and this creates numerous opportunities
for internal red teams, not to mention slightly more nefarious crews. Today, we're
going to look at two very quick ways to run arbitrary code on the machines of
developers (and build systems) before moving on to mention further avenues for
exploration.

<!-- more -->

# Before we go on...

## Disclaimer
I am not proposing that you use what I'm going to cover for nefarious purposes,
this post is purely educational so that developers can be aware of the risks
when using third-party code (open source or otherwise).

## Required knowledge
For the rest of this post, I'm assuming that you have a working knowledge of
bash, terraform and are familiar with terraform modules. If not, here are some
decent sources for the terraform side of things:

 - [Intro to Terraform](https://www.terraform.io/intro/index.html)
 - [Using Terraform Modules](https://learn.hashicorp.com/terraform/modules/using-modules)

# Running code via `local-exec`
The first way that we're going to look at running arbitrary code is by using
[`local-exec`](https://www.terraform.io/docs/provisioners/local-exec.html). From
the terraform documentation page:

```md
# local-exec Provisioner

The local-exec provisioner invokes a local executable after a resource is created. This invokes a process on the machine running Terraform, not on the resource. See the remote-exec provisioner to run commands on the resource.
```

So what is this saying? This is saying that for any terraform `resource`, we
can run arbitrary code locally (for remote execution, see the [`remote-exec`](https://www.terraform.io/docs/provisioners/remote-exec.html)
provisioner). Now, the docs do call out that provisioners should only be used
as a last resort, but we're going to use them for our own fun today, let's look
at an example of how we'd do this:

```hcl
# This is the pre-existing `null_resource`, see the docs here:
# https://www.terraform.io/docs/provisioners/null_resource.html
resource "null_resource" "cluster" {
 provisioner "local-exec" {
    command = "${path.module}/entry.sh"
  }
}
```

Here, we're creating a `null_resource`, which won't actually create anything. By
using the null resource, we can run arbitrary code without worrying about
impacting any existing terraform state or having any other side effects.

So what does it look like then we make a plan with this terraform code? Let's
run `terraform init && terraform plan -out=null_resource.tf.plan`:

```sh
------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + module.demo.null_resource.cluster
      id: <computed>


Plan: 1 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------
```

If we're trying to hide our actions, this isn't ideal - but in a larger module
import, with potentially 10s to hundreds of resource creations, modifications or
deletions, you'd be hard pressed to find this individual line. Note, that we
haven't yet run our script, `entry.sh`. So let's apply our plan (`terraform apply null_resource.tf.plan`).

```sh
module.demo.null_resource.cluster: Creating...
module.demo.null_resource.cluster: Provisioning with 'local-exec'...
module.demo.null_resource.cluster (local-exec): Executing: ["/bin/sh" "-c" "/<redacted path>/.terraform/modules/73a34264d981f034e09486899c66805b/entry.sh"]
module.demo.null_resource.cluster: Creation complete after 0s (ID: 4162867750964960682)

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

If you'd like to see a module that accomplishes this, check out
[this example here](https://github.com/ttacon/terraform-malicious-null-resource).


# Running code via external data sources
One of the other main concepts in Terraform are [data sources](https://www.terraform.io/docs/configuration/data-sources.html).
A big difference here, is that if we're using a `provisioner` via a `resource`
or from inside a `module`, we have to wait until the `apply` stage to execute
our code. But data sources are items that pull information to be used later in
our Terraform code, so via them, we can execute code at the `plan` stage. If
all of this lifecycle discussion is confusing, don't worry, we'll get into
this below.

Back to running arbitrary code, so how can we pull this off via data sources?
Luckily, Terraform provides an [`external` data source](https://registry.terraform.io/providers/hashicorp/external/latest/docs/data-sources/data_source)
that will allow us to run any program that we want. This could then look like
this:

```sh
data "external" "entrypoint" {
  program = ["bash", "${path.module}/entry.sh"]
}
```

Where, again, `entry.sh` is a script that we bring along for the ride. With this
code, when we `terraform init && terraform plan`, we get:

```sh
terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.

data.external.entrypoint: Refreshing state...

------------------------------------------------------------------------

No changes. Infrastructure is up-to-date.

This means that Terraform did not detect any differences between your
configuration and real physical resources that exist. As a result, no
actions need to be performed.
```

Looks like nothing happened, but our script has actually run!
[Here's a module](https://github.com/ttacon/terraform-malicious-example)
that will help make this super clear.


## Gotchas
A quick note on the `external` data source, there are a few gotchas that
I stumbled over because I didn't read the docs clearly enough:

 - You can provide any JSON object via the [`query` property](https://registry.terraform.io/providers/hashicorp/external/latest/docs/data-sources/data_source#query)
 - Your program **MUST** return valid JSON ([see docs here](https://registry.terraform.io/providers/hashicorp/external/latest/docs/data-sources/data_source#external-program-protocol)).


# Why do we care?

When we're running red team exercises, these techniques can be very useful to
gain an initial foothold in a system - whether or not we running from an
authenticated or privileged vantage point. Specifically, let's look at two ways
we can use the above techniques to enter into a system.

## Local developer development

Depending on an organization's operational maturity, there might be AWS
credentials, SSH keys, etc lying around on a developer's machine. While a
developer is developing new Terraform code, we can use the data source technique
to get onto their machine. If a company doesn't have an automated build system
for plannign and applying Terraform changes (hello [Terraform Cloud](https://www.hashicorp.com/products/terraform/) and
[Atlantis](https://www.runatlantis.io/)), then we can use the our `resource`
based technique as well.

Hopefully, since we're on a developer machine, they're using expiring STS tokens
and not more permanent and static credentials.

## Build systems

Here's where this rabbit hole started for me. We often talk about zero trust
systems, yet as developers we often treat our build systems with a high degree
of trust - this is a **BAD** idea. Instead, we should consider our build system
as a place that could be infected by almost anything and build in appropriate
defenses, including, but not limited to:

 - Turn off egress except for specifically proxied content
 - Use our own internal registries with pinned, scanned content
 - Run everything in containers with tightly controlled file system access and very limited environment variables
 - Periodically purge build caches (if you use them)
 - Process fingerprint our build runs (more on this in a later post)


All of this is to say that we can use both of the above techniques to gain
privileged access unless an organization maintains top notch security practices.


# Ways to identify issues

Really only good security hygeine will help here - scan new modules and code for
use of the `local-exec` provisioner (or the `remote-exec` provisioner, which we
didn't talk about) and use of the `external` data source. Both of these can be
entirely valid in the right scenarios, **and** we should always verify their
usage.


# Future research areas

Next up, I'll be sharing how you can use malicious Terraform providers as
another foothold. Until then!


