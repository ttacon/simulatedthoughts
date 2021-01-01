+++
title = "Security awareness training that actually helps"
date = 2020-07-17
+++

# Security awareness training

For most people, the phrase "security awareness training" conjours memories of
videos you can't fast forward through, slides you don't care about and just
plain boredom. The sad thing is, it security awareness training doesn't have
to be this way! So today I want to share my thoughts on how to build a security
awareness training program that:

 1. Isn't incredibly boring (and so purely for compliance purposes)
 2. Will stick with your staff for very, very long periods of time

<!-- more -->

# First things first: NIST 800-50
If you've never read [NIST 800-50](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-50.pdf),
you should. While it's certainly dry, and fairly old, it contains invaluable
information. So go read it and then come back here.

# How to make security awareness training fun and impactful

## Make it real

It's easy to run through examples that you copy-pasta from the internet -
here's what the definition of `spam` is, here's the definition of `encryption`.
The thing is, none of that is useful without a connection that makes it
personal, or at least more tangible, for each indvidual completing security
awareness training. So how do we make it more real? Share real life examples!
Here are a few that I personally share in the most recent new-hire/shared
background security awareness training that we do:


### Make it real (1): Full disk encryption

Full disk encryption (FDE) is table stakes for any staff used machine. In most
organizations with device management, we can ensure that FDE is enabled by
default for staff machines. However, I've seen scenarios where individuals,
some with *very* sensitive access, do not have FDE enabled or have somehow
disabled it. In these situations, this is often because they view enabling FDE
as a hassle - why spend the time to do it when they have other things to do?

This is why helping teammates understand the importance of FDE is critical, so
let's make it more real. One good example is helping staff with Mac's understand
just how quickly their information can be stolen without FDE. An example that I
like to use here is to show, or walk them through:

 - Rebooting their computer
 - Ensuring that it boots in `Target Disk Mode`
 - Connecting my laptop to theirs
 - Beginning to copy all of their data

This example is really powerful, because how long does this take? If you don't
have a lot of data, then it doesn't take too long. Let's say five minutes for
argument's sake. How long does it take you on average to go to the bathroom in
a coffee shop - if it's more than five minutes (perhaps there's a line?), then
you'd never know I'd stolen all your information.

Another, fun, example I like to use is the case of Dread Pirate Roberts (the
creator of the Silk Road) and how the FBI built their case against him. Here,
I find it fun to give folks an elaborate run down of the day that Dread Pirate
Roberts [was arrested](https://www.wired.com/2015/01/silk-road-trial-undercover-dhs-fbi-trap-ross-ulbricht/),
how the FBI were able to [get access to his machine](https://www.forbes.com/sites/sarahjeong/2015/01/22/the-dread-pirates-diary/#53aa65a042cf) in an
unencrypted state, and how the case against him would have been _much_ harder
if they hadn't tackled him away from his computer after he'd decrypted the disk.

Do note, and I call this out in that part of the training, I'm not advocating
for you, or anyone else, to become an underworld drug lord. I'm purely pointing
out the power of FDE.


### Make it real (2): SMS based MFA

Where other security methods exist (e.g. Yubikey, TOTP, etc), SMS based MFA
should never be used. Why is it important to call this out? When I first rolled
Duo MFA out at a company, I found that many staff still preferred to used SMS
based MFA. Frankly, this scared me. SMS based MFA is extremely vulnerable to
attack, when compared to hardware keys or even TOTP. A story that I use here
is a very personal one.

Last winter, while my family and I were playing Terraforming Mars (great game if
you've never played it), I got a text that my partner's phone number was now
associated with a different SIM card. Of course, we immediately freaked out and
called our telephony provider (thankfully, my phone still worked). They claimed
that it was an accident and resolved the issue within the next 30 minutes.
However, this created a massive headache for us - many institutions, including
financial ones, only support SMS based MFA (why? I've no clue, but don't get me
started). This meant that we had to spend the rest of the evening checking every
account that she had SMS MFA enabled on. Thankfully, none of her accounts had
been compromised, but (SIM swapping)[https://en.wikipedia.org/wiki/SIM_swap_scam]
is a *major* issue, and if you have a choice, choose not to use SMS based MFA.

### Make it real (3): More for later

There are a ton of other great stories and examples, if you're interested in
them, let me know! Send me a tweet or hit me up, happy to share them - it's
eye opening for people to understand the dangers of Chrome extensions,
developer tooling, rogue WiFi networks and more.
 

## Teach people how to do it

This might be a bit more controversial, but I've found that teaching staff how
to pull off some of these hacks or security attacks is the best way to help them
understand how to keep themselves safer. This is a pretty new focus area for me,
so I'll share one that I've done, how it went, and what I plan to do next (we do
security awareness training every six months).

Do note, that depending on the different audiences, you need to tailor these
trainings very carefully - you want non-technical staff to come away with an
equal strong understanding as your technical staff.

### Teaching (1): [Spear] Phishing
Phishing is something that most people view as a very basic attack and security
risk, and yet it's still one of the most successful attack vectors that there
is. So how can this be true? This is because most spear phishing attacks are
highly sophisticated and often, unfortunately, go unnoticed. This is why it's
so important to help our staff understand how exactly spear phishing attacks
are executed.

What I like to do here, is use [Gophish](https://getgophish.com/), an open
source phishing platform, to run training labs with groups of staff. In those
groups, I have staff attempt to build a spear phishing attack against me,
personally. Why me? I want them to experience truly gathering OSINT and
launching a spear phishing campaign, and I don't want to make anyone feel
uncomfortable - focusing the target on my back helps in this regard (even if
it does mean I get some  _very_ interesting emails for a little while).

The key here - by helping staff design a phishing campaign, it really drives
home the major red flags to look for that identify an email as a phishing email.
We're trying to help people keep themselves safe, so we're not teaching them to
be expert phishers - we're teaching them just enough to be able to better
protect themselves.


### Teaching (2): What's next

There are a lot of fun future areas to help train staff to be safer, I'm going
to be building my next training from one of these areas:

 1. MITM/dangerous WiFi networks
 2. Spyware Chrome Extensions
 3. Installing unverified software


Any of these areas have great red flags to help staff focus on to learn to be
aware of, so they should all be a lot of fun!

 
