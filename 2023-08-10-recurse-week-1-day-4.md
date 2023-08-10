---
summary: >-
  I'm about to participate in a "half batch" at the Recurse Center in Brooklyn.
---

# Recurse Center, four days in

Since arriving in New York last Saturday, I have settled into my AirBnB, did
some prep work, and most importantly started my batch.

"Prep" included essential things such as modifying my self-updating [GitHub
profile][profile] to be more RC specific, with statuses reflecting the
[self-directives] and the random activities in the README body being tailored
to what I think I'll be doing.

I also really wanted to add an Atom feed to my static site generator, and it's
what I spent most of my actual programming time on in the last few days. The
[*Introduction to Atom*][atom] is quite readable and has all I needed. I ended
up generating a feed with just summaries instead of full post content, even
though I quite dislike it when websites choose to do that; it would have been
too much work for right now to add posts in their entirety. I did add *some*
polish in the form of setting a feed icon that shows up in, e.g., Feedly, and
hopefully other readers, too.

Implementation-wise, I misappropriated [yq] to build the feed basically in
YAML, but then print it in XML. XML-specific things are supported as special
field names, as in

```sh
$ yq -n -o=xml '
  {
    "title": {
      "+@type":"html",
      "+content": "The title"
    }
  }
'
<title type="html">The title</title>
```

On the plus side, [it works][feed]!

On the negative side, this is a bit of a cop-out where I spend time on
something I'm already very familiar with, instead of pushing myself. My excuse
was that I really want the feed to be there so I can learn in public and hook
my blog up to the RC-internal blog feed, but now that that's done, I want to
stop fiddling with the blog and just use it to publish stuff.

## Things other than programming

Monday and Tuesday, the hub wasn't open yet for people starting a batch, so I
went to a co-working space near the hub. There wasn't a *ton* going on yet,
mostly two meetings per day with intros to RC, everybody introducing
themselves, a pairing workshop, and a tour of the software used at RC. It was
still pretty exhausting, and time went by incredibly fast. (That hasn't changed
at all since then! :sweat_smile:)

Yesterday, we finally got access to the hub. I guess there were about 50 people
in yesterday, with a mix of Recursers starting the second half of their batch,
and others who just started this week with me. The place was buzzing with
energy, I had a lot of great conversations, and was very tired in the evening.

Overall, it's overwhelming, but in a good sense. I still have to figure out a
few things:

- Doing things at a sustainable pace: I probably can't work at the hub until
  8.30 pm every day and then go to my AirBnB and continue working until
  midnight.
- Working on the right things. My "real" project has received very little love
  from me so far, because of important things like the feed for the blog! Also,
  first week things. I'm giving myself half of a pass for this.
- General fear of missing out: should I do more pairing? Should I be more
  social? Should I be working more on my project? Since you can only do one
  thing at a time, feeling like you should actually be doing one of the others is
  quite easy. (And not helpful!)
- Not totally fall off the fitness wagon: I want to do lots of shorts runs,
  combined with enough physio to keep my calves and IT band at bay. And eat not
  terribly.

In this spirit, I want my next post to be about non-zero progress on the VR
project.

[profile]: <https://github.com/bewuethr>
[self-directives]: <https://www.recurse.com/self-directives>
[atom]: <https://validator.w3.org/feed/docs/atom.html>
[yq]: <https://github.com/mikefarah/yq>
[feed]: </feed.xml>
