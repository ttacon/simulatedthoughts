+++
title = "Education vs Training"
date = "2021-01-01"
+++

# Education vs Training

I often talk about the best teacher being failure, and so a goal of mine, when
teaching or mentoring, being to set others up to fail softly. This framing helps
me check my own personal desires to see a project or goal succeed, and instead
to focus on the personal growth of the individual that I'm working with.

This has felt like a pretty solid perspective to me, until I was recently
reading `The Art of Doing Science and Engineering` by Richard Hamming. As an
aside, I'm barely 100 pages in and it's already helped me learn numerous new
perspectives - if you enjoy learning about learning, I highly suggest it. Back
on track, in the first few pages, Hamming distinguishes Education and Training
from each other:

```
Education is what, when, and why to do things.
Training is how to do it.
```

This is a framing that I keep coming back to in my mind, in particular, helping
me to articulate and explore the gap that I feel exists in the software world
today.


Software developers of traditional backgrounds usually pursue Computer Science
or Computer Engineering degrees (the naming varies program by program). I myself
went this route, and I'll be honest, while I personally use many of those
fundamentals that I learned back then nowadays, I didn't for a very long time,
and many people that I know don't use any of them. I recall learning about
concepts that were highly interested to me at the time, but certainly have never
helped me hit a project milestone or sprint story point goal. It was only once I
got into industry and began "doing" things that I felt I really began to learn.
Now I acknowledge that this could have been due to me never doing a single 
internship during school (I had thought that I wanted to go into medicine for the
first four years), but as far as I'm aware, my story and perspective on this are
far from unique. My education was helpful, but my training was far more so, and
I think a lot of this is because my education was to be a computer scientist, but
that's not at all what I am today.


Instead, it would have been more helpful to have learned about how to setup
nginx (including why I needed it) or how to use an observability tool (although
the term didn't exist in it's current usage while I was in school). This is
something that most folks are looking for in mentorship opportunities at
companies. They're looking for many other things, too, but while they are doing
their jobs (which for the vast majority of people could be considered
compensated training, or an apprenticeship program in other parlance), they're
looking to understand what, when and why to do the things that they're doing.

Eh, I'm not sure that captures what I'm thinking at all, in fact I'm finding
myself disagreeing with it now (mostly around the phrasing as an apprenticeship
program, which is something I do strongly feel we need more of as an industry).
So perhaps an example would be useful here. 

Refactoring. Refactoring is a mythical opportunity that many companies talk
about and few put into practice. Refactoring is the opporunity, in its raw 
essence, to make something better (this is not restricted to technical
components, organizational structures would do well with refactoring, too).
In companies that do practice refactoring, we teach our employees hwo to do it.
Here's a good way to extract a common function. Here's a good way to add a layer
of abstraction to support an experimental or plugin based approach. Here's a
good way to make a function more testable. But we never, ever:

 - Explain what refactoring is. In my early career, to me, refactoring meant
   moving code around for that sole, myopic purpose. No wonder management hated
   hearing that the code needed to be refactored. Instead, we should be
   explaining the numerous benefits of refactoring, and the guiding principle
   of desiring to make our systems better.
 - Explain when refactoring is appropriate. I may be in the minority here, but
   I believe that we should always be refactoring code in an area that we're
   working in. If we're not, we're only building tech debt for later.
 - Explain why we do refactoring. This is one where I think leaders truly drop
   the ball. Refactoring code has a direct impact on implementation quality and
   speed. It's a team's best friend, and there isn't a good reason not to do it.
   The only reason that I've ever heard is that it takes more time in the short,
   which, for me, is not a good reason not to do it.
   
Yeah, that example is not terrible. Hopefully it gets across this concept that I'm
trying to work through how to help better implement at my current place of work.
For some engineers, in the past, I'd build them sets of scenarios to test out new
knowledge in. I recall some specifically around learning how to setup and
configure haproxy and nginx, the differences between them and how to choose the
correct tool when needed. This is something that I think that I want to return
to more this year, hopefully these can be easy to open source and others can
learn from them, too.

I'm not sure this was coherent, but I guess that the point that I'm trying to
convey in this rambling is that we need to help educate our peers in our industry.
While we pass on our training to others, we need to help convey the education and
learnings behind it, and if we can't, we should strive to dig deeper and educate
ourselves more as well.








