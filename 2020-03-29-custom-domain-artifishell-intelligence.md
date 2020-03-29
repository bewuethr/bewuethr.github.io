# A custom domain for ArtifiShell Intelligence

A new name *and* a custom domain? Yes!

"ArtifiShell Intelligence" narrowly won out over "Shell we dance?" (in wide use
for, well, *dancing* events such as [The Netherland's largest dancing
competition][1]), "Terminal Shellness" (bit bleak), "Super Fish Shell" (almost
perfect, except that there is the [fish shell][2]) and "BenefiShell Musings".
I thought something with "Prompt" would totally work, too, but couldn't come up
with a single idea for it.

As an added bonus, googling for "artifishell" led me to [this great Instagram
account][3].

At some point, I'll even create a pretty header for this new title.

[1]: http://www.shellwedanceevent.nl/Shell_We_Dance_Event/Home.html
[2]: https://fishshell.com/
[3]: https://www.instagram.com/mofart_photomontages/

## GitHub Pages with a custom domain

I've owned *benjaminwuethrich.dev* for more than a year, and now I'm finally
using it. I wanted both the apex domain and *www.benjaminwuethrich.dev* to work
and require HTTPS, but as it turns out, that's not totally straightforward.
I read through all the corresponding [instructions][4] and assumed that this
should work:

- Set custom domain in settings to *www.benjaminwuethrich.dev* (generates
  a `CNAME` file)
- Create CNAME record in Google Domains that points *www.benjaminwuethrich.dev*
  to *bewuethr.github.io*, my old default domain
- Create A record in Google Domains that points to the GitHub Pages IP
  addresses listed in the [help pages][5]

However, this resulted in *www.benjaminwuethrich.dev* working, while
*benjaminwuethrich.dev* complained about a missing security certificate. There
is a [long thread][6] in the GitHub Community Forum with people having similar
problems, and the inofficial GitHub bug tracker has an [entry][7] for it as
well. It affects even sites like *commonmark.org*!

After going back and forth with what's in the `CNAME` file and in the repo
settings (`www` or not), having `www` in both places ended up working. As
pointed out in the thread linked above, this might break in three months when
only the cert for *www.benjaminwuethrich.dev* gets renewed, but not
*benjaminwuethrich.dev*. I'll see; everything works for now, and I won't touch
it!

[4]: https://help.github.com/en/github/working-with-github-pages/configuring-a-custom-domain-for-your-github-pages-site
[5]: https://help.github.com/en/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain
[6]: https://github.community/t5/GitHub-Pages/Does-GitHub-Pages-Support-HTTPS-for-www-and-subdomains/td-p/7116
[7]: https://github.com/isaacs/github/issues/1675
