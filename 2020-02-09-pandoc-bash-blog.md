# Pandoc Bash Blog

This blog has seen very little love for a while; not because I had no ideas
what I could write about, but because I didn't do it.

This must have been due to "not invented here" syndrome as I was using a
[Jekyll Now][jekyll] based blog; today, I've started writing my own little blog
generator. So far, it's a shell wrapper around [pandoc] and does little more
than this:

[jekyll]: https://github.com/barryclark/jekyll-now
[pandoc]: https://pandoc.org

```bash
for f in *.md; do
    pandoc \
        --from=markdown \
        --to=html \
        --output="artifacts/${f/%.md/.html}" \
        --standalone \
        "$f"
done
```

There's a little snippet that builds an index page:

```bash
{
    printf '%s\n\n' "# Benjamin's blog"
    for f in ????-??-??-*.md; do
        read -r _ title < "$f"
        printf -- '- [%s](%s)\n' "$title" "${f/%.md/.html}"
    done | tac
} > index.md
```

and that's it! The rest is some Git contortions to get the generated files into
the right branch, assuming a blog published on GitHub pages. The pandoc
defaults are quite alright, but I'm looking forward to endless fiddling with
it. I'll spin the generator out into its own repository eventually, but I'll
keep track of the progress here.
