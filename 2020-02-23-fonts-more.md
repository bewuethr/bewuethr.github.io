# Fonts and more

Pandoc Bash Blog has experienced a few upgrades since the [addition of CSS][1].
Some are conveniences when using `pbb` on the command line, some change the
appearance of the published result.

[1]: https://bewuethr.github.io/2020-02-16-css-for-pbb.html

## Tab completion

`pbb` knows only a bunch of subcommands (five, to be precise); now, when typing
`pbb`, tab completion suggests them. That is, if the completion file is in the
right place, which a (forthcoming) installation script has to take care of.

## Language setting

Pandoc's default HTML has an empty `lang` property in the opening `<html>` tag,
which is [bad for screen readers][2]. Previously, `pbb` would default to
inserting `en-CA`; now, it tries to infer the language from environment
variables. This is surprisingly non-straightforward; I settled on the same
priority order as [gettext][3], which collapses to this beautiful declaration:

```bash
lang=${LANGUAGE:-${LC_ALL:-${LC_MESSAGES:-$LANG}}}
```

Some special characters have to be replaced to make for a valid language tag;
if none of these variables have a value, `pbb` defaults to `en-US`. I guess
eventually, there should be an option to make the language a configuration
value.

[2]: https://youtu.be/NP94u7y_KkQ
[3]: https://www.gnu.org/software/gettext/manual/html_node/Locale-Environment-Variables.html#Locale-Environment-Variables

## Local server

To look at the built site before deploying it, I used to use the Python
one-liner that spins up an HTTP server on localhost; for convenience, this is
now done by `pbb serve`, which just calls

```bash
python3 -m http.server --directory artifacts
```

under the hood.

## Favicon

`pbb build` now checks if there is an image with a name that matches
`favicon.*` in the `assets` directory. If so, it gets converted to a 32x32 PNG
file and included as the favicon on each page.

The actual conversion is quite flexible: as long as ImageMagick can handle the
input format, the favicon gets created. The input file can even be an animated
GIF; in that case, the first frame is used. The command is this one:

```bash
convert "${infile[0]}[0]" -resize 32x32^ -gravity center \
    -background none -extent 32x32 artifacts/favicon.png
```

`infile` is an array of all files matching `favicon.*`; at this point, there's
already been a test to make sure that its length is 1.

The rest of the command takes care of all the details:

- Extract the first frame in case there are multiple with the `[0]` index; this
  simply does nothing if there aren't multiple frames.
- Resize to fill (and potentially overflow) 32x32 pixels, from the centre of
  the input image with `-resize 32x32^ -gravity center`
- Preserve transparent background for PNG inputs with `-background none`
- Crop outside of the 32x32 square with `-extent 32x32`

## Web fonts

I wanted to have pretty fonts instead of the system defaults. A convenient way
for doing so is to include fonts via [Google web fonts][4]. I want `pbb` to be
minimal and privacy respecting, and the pages should load fast, so I wondered
if I should host the fonts myself or fetch them from Google.

Regarding the privacy aspect, Google [says][5] they only log aggregate data,
and in a recent [discussion on Hacker News][6], a member of the original team
that launched Google Fonts [says][7] that there are lower hanging fruits than
fonts if you really wanted to track users. For whatever that's worth, I guess.

Regarding performance, I found a [great article][8] covering about everything I
could have wondered. It's not actually that clear cut; for example: using
Google instead of self-hosting gets you updates to the fonts and lets you
benefit from technological improvements such as variable fonts once they become
more commonplace, without having to change anything at all. On the other hand,
Google can change your font and [break your page][9] in the process.

Google is also smart in how it delivers the font: the CSS returned contains a
font format tailored to the user agent, and the fonts contain hints or not
based on the OS. Replicating this when self-hosting is not trivial.

As a negative, because requesting CSS from a third party is involved, rendering
is blocked until all the CSS has been loaded. This was the reason for a
substantial performance improvement for the author of the article and why he
ended up self-hosting.

In the end, I decided to load the fonts from Google for now, as it keeps my CSS
a lot simpler. I optimized the requests as far as possible with a `preconnect`
resource hint, and I suspect for my tiny page, the difference isn't noticeable
anyway. Later on, I may want to self-host the fonts, though.

Until then, Google serves my fonts of choice, [Source Sans Pro][10] for text
and [Source Code Pro][11] for code.

[4]: https://developers.google.com/fonts
[5]: https://developers.google.com/fonts/faq#what_does_using_the_google_fonts_api_mean_for_the_privacy_of_my_users
[6]: https://news.ycombinator.com/item?id=22368247
[7]: https://news.ycombinator.com/item?id=22370250
[8]: https://www.tunetheweb.com/blog/should-you-self-host-google-fonts
[9]: https://github.com/google/fonts/issues/1137
[10]: https://fonts.adobe.com/fonts/source-sans
[11]: https://fonts.adobe.com/fonts/source-code-pro
