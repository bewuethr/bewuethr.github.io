# Datestamps

`pbb` now adds dates to all the post titles on the index page and to the titles
of individual posts. The date is simply extracted from the filename; in the
`md2html` function, I check if the filename looks like it starts with a date,
and if so, I add the date to the markdown:

```bash
{
    if [[ $file == ????-??-??-* ]]; then
        printf '%s\n\n' "${file:0:10}"
    fi
    cat "$file"
} | pandoc "${args[@]}"
```

where `${file:0:10}` is a parameter expansion that extracts the first ten
characters from the filename and `args` is an array containing all the pandoc
options I need.

`pbb` now clocks in at 157 lines and is still quite readable (at least for me,
shortly after writing it); I'm thinking about adding [Bats][bats] tests soon to
make it easier to see if something breaks when I add new features.

[bats]: https://github.com/bats-core/bats-core
