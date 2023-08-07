# Attending the Recurse Center

Starting tomorrow, I'll attend a "half batch" of six weeks at the [Recurse
Center][rc], a programming retreat. After lots of research and prep work, I
booked some accommodation and flights, and arrived in Brooklyn yesterday. I'm
very glad my combined Swiss--Toronto background has prepared me for high cost
of living, but New York certainly puts all that into (expensive) perspective.

For the first two weeks, I'll stay in Carroll Gardens, and then for three weeks
in Prospect Heights. I still have to figure out something for the last week,
but I hope that'll be easier, being here and all.

The first two days, tomorrow and the day after tomorrow, have the hub not yet
open for people starting their batch, to make sure everybody's onboarding
experience is the same, i.e., online. The hub was closed for over two years,
and all batches took place online; it was [reopened in June][blog] for a trial
period of about three months---and recently, it was decided to keep it open
until further notice!

Batches are now a mix of people attending virtually and in-person, and I wanted
to wait until in-person was possible again. The trial also lined up well with
my five-year anniversary at [work], which gives me some extra time off in the
form of a sabbatical. As far as I can tell, there is going to be a good number
of in-person Recursers.

[rc]: <https://www.recurse.com/>
[blog]: <https://www.recurse.com/blog/189-a-new-kind-of-retreat>
[work]: <https://www.koho.ca/>

## Project ideas

A big challenge was sifting through my long list of projects and selecting some
that would be a good fit: challenging, but not so hard that I'd only get to
"hello world" in six weeks, exciting, enabling me to learn new things...

To give you an idea of the unfiltered selection, here's what the list initially
looked like:

- Learn C and writing a shell
- [Build an 8-bit computer from scratch][8bit]
- Learn Ruby to hack on Jekyll plugins for the [Swiss Club website][sct]
- [Build Git from scratch in Ruby][buildgit]
- Extend my [Bash raytracer][bashrt], following [*Ray Tracing in One
  Weekend*][rtwknd] or [*The Ray Tracer Challenge*][rtchallenge]
- Learn data visualization using [D3.js] or similar
- Do [*Linux from Scratch*][lfs]
- Build a CRUD web app end-to-end, including frontend, backend, infrastructure;
  for example, a membership management tool for an association or club
- Build an app using QR codes to remember where stuff in a warehouse or similar
  is stored (codes on shelves map to list of things on the self)

[8bit]: <https://eater.net/8bit/>
[sct]: <https://https://swissclubtoronto.ca/>
[buildgit]: <https://shop.jcoglan.com/building-git/>
[bashrt]: <https://github.com/bewuethr/bash-raytracer>
[rtwknd]: <https://raytracing.github.io/>
[rtchallenge]: <http://raytracerchallenge.com/>
[d3.js]: <https://d3js.org/>
[lfs]: <https://www.linuxfromscratch.org/>

## Choosing two things

In the end, I settled on two projects: a main project, and a less challenging
fallback project.

For the fallback project, I'm going to start reading the [Pickaxe
book][pickaxe] to learn Ruby, and after a while start the [*Building
Git*][buildgit] project.

But for the main course, and notice how that wasn't on the original list
:grimacing: I want to work on a virtual "physical environment". I'm envisioning
a three-dimensional room with objects in it that behaves as if it were attached
to the viewer, which could be a Google Cardboard, or a VR headset; imagine a
ball in the room, which should roll towards the viewer when leaning back, to
the right when tilting the viewer clockwise, and so on.

I think this could be web-based, using whatever magic there is when you can
select "view in VR" on a website. This project would combine a lot of things
for me:

- My original computer passion, physically-based simulation (that's roughly
  what I studied in university, and worked on for a few years in my first job)
- Something visual, where progress translates immediately into a new thing to
  look at
- Frontend web tech and new device APIs that I've never even touched
- Open-endedness; there is always more one can add to such a project, so
  chances I'll be "done too early" are slim
- Can start with simple prototypes: 3D/VR can come later; starting out with a
  2D canvas reacting to phone sensors is probably easier to begin with

[pickaxe]: <https://pragprog.com/titles/ruby5/programming-ruby-3-2-5th-edition/>

## But who knows

Two of the three [self-directives] of Recurse Center are "build your volitional
muscles", and "learn generously". (The third one is "work at the edge of your
abilities", which I think the previous section covers.)

I understand the "volitional muscle" as becoming good at realizing what excites
me the most, and then acting on that realization. In other words (and as pretty
much all alumni I've spoken with confirmed), the projects you planned working
on might not be what you end up working on in reality. Especially for me, doing
just half a batch, I think it is thus important to focus on my initial project
to reach a "breaking point" sooner rather than later. The one scenario I want
to avoid is realizing that I don't want to work on a project a few days before
my batch ends.

And learning generously is about sharing, pairing, demo-ing... one way I plan
on doing that is by blogging regularly (hence this post!). Since this is still
based on my own, very minimal static site generator, there's no RSS feed to
subscribe to---yet! I'll try and add one during the first week or so.

[self-directives]: <https://www.recurse.com/self-directives>
