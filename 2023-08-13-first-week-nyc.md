---
summary: >-
  Assorted thoughts after one week in New York
---

# First week in New York

I'm still trying to figure out what should go into these posts here, and what
into my daily written checkins on Zulip, the RC-internal messaging platform,
without too much duplication. Trying to take a big picture approach here
:slightly_smiling_face:

## Logistics

Getting around is fairly convenient with the subway, which is never further
away than a few minutes of walking. My morning/evening commute is just three
stops, and about 20 minutes total.

Paying for the subway is as simple as tapping your phone or credit card;
there's even a weekly cap after 12 trips in the same week on the same card,
though at about C\$3.70 per trip, we're not exactly talking "cheap".

I tried using a share bike last weekend, but it wouldn't let me unlock, neither
with credit card as the payment method, nor with prepaid Lyft money. Maybe that
station was offline or something; I'll try again.

It's not possible to buy bike passes for less than a year, and a single trip is
about C\$6, so not really a commuting option. I'm considering buying an annual
pass (C\$275), and then sell it to another Recurser at the end of my batch. And
they can sell it at the end of theirs, and so on.

My AirBnB is fine, though the Brooklyn Queens Expressway right in front of the
building isn't what one would call a source of silence (my place is where the
blue dot is):

![Brooklyn Queens Expressway and my AirBnB][airbnb]

[airbnb]: <images/2023-08-13-expressway.webp>

In addition to the eight expressway lanes, there's also a busy street under it,
and on top of just traffic, there are some pipes under the expressway that,
when excited "correctly", clatter for a few seconds really *really* loudly.

I'm moving to another, much nicer place in a week, though!

## Trying to be healthy

I'm not going to cook at the current place, but maybe at the next one. It's
hard to resist the ubiquitous temptation of pizza slices for breakfast, lunch,
and dinner, though since I've discovered Trader Joe's, healthy not-so-expensive
food is a thing.

I'm trying to do lots of short runs and enough physio to enable the runs; I've
managed three this weeks, though two were on the weekend. I can definitely do
better here.

## Project progress

I wrote in the [last update] that I want this post to report on progress on my
main project, the physically-based virtual toybox (just "VR toybox" from now
on, I guess), and there is something to report on!

[last update]: <2023-08-10-recurse-week-1-day-4.html>

It's small enough to live in its entirety in this post, so here goes:

<style>
  canvas {
    border: 2px solid;
  }
</style>
<canvas height="500px" width="600px"></canvas>
<script>
  function init() {
    let canvas = document.querySelector("canvas");
    let ctx = canvas.getContext("2d");
    ctx.textAlign = "end";
    ctx.fillStyle = "black";
    return [canvas, ctx];
  }

  function drawBall(ctx, x, y, r) {
    console.log(`Drawing: ${x}, ${y}, ${r}`);
    ctx.clearRect(0, 0, ctx.canvas.clientWidth, ctx.canvas.clientHeight);
    ctx.beginPath();
    ctx.arc(x, y, r, 0, 2 * Math.PI);
    ctx.fill();
    ctx.fillText(`x: ${x}`, ctx.canvas.clientWidth - 10, 20);
    ctx.fillText(`y: ${y}`, ctx.canvas.clientWidth - 10, 30);
  }

  function frame(time) {
    drawBall(ctx, x, y, r);
    requestAnimationFrame(frame);
  }

  addEventListener("keydown", event => {
    let step = 2 * r;
    switch (event.key) {
      case 'ArrowRight':
        x = x + step > ctx.canvas.clientWidth ? x : x + step;
        break;
      case 'ArrowLeft':
        x = x - step < 0 ? x : x - step;
        break;
      case 'ArrowUp':
        y = y - step < 0 ? y : y - step;
        event.preventDefault();
        break;
      case 'ArrowDown':
        y = y + step > ctx.canvas.clientHeight ? y : y + step;
        event.preventDefault();
        break;
    }
  });

  let [canvas, ctx] = init();
  const r = 5;
  let x = ctx.canvas.clientWidth / 2, y = ctx.canvas.clientHeight / 2;
  drawBall(ctx, x, y, r);

  requestAnimationFrame(frame);
</script>

It's a dot that can be moved around with arrow keys. Or, I should say, it's a
(very simple) scene where the one interactive element can be controlled by
direct input, where the "model" is limited to "dot can't leave the canvas".
Iterations on this will be

- The input is provided by device sensors
- The dot behaves physically reasonable
- The scene is three-dimensional
- The scene can be viewed in a VR viewer
- The scene is more complex than a single item (though I'll be happy with a 3D
  single-item scene, too!)

I've also started with my second project, learning Ruby, which currently just
means "I've started reading the Pickaxe book".

## Some insights

It's very strange to be able to show up in the morning and immediately start
working on whatever I want to work on. I have to unlearn that there are a few
hours of pull request reviews, design documents to look at, inboxes to sift
through, with "actual" work[^1] often not starting before 2 pm or so. Here,
it's right away! Sure, maybe plan your day, but that's it. I have yet to take
full advantage of this.

On Friday, we've done a workshop to reflect on what we want to work on, and
why. My main priorities haven't changed, but I have down-ranked a few of the
ideas I had when thinking about RC projects.

And lastly, conversations with other Recursers, be it over lunch, at the coffee
machine, or just anywhere, keep being a source of joy and inspiration. I truly
appreciate the system that manages to assemble such a group of people, and I
feel very lucky to be part of it. The last time I felt similar about a group
was probably when I did my exchange program with [UNITECH], with a similar
active inclusion of alumni---and that was quite a while ago!

[^1]: I know that these things *are* actual, and useful, work. I guess the main
difference is that I'm not totally in control of them; some days, there are
many PRs, some days, there are fewer, but all have to be looked at.

[unitech]: <https://www.unitech-international.org/>
