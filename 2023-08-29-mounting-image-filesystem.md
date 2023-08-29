---
summary: >
  My "Impossible Stuff Day" project, mounting an image as a filesystem
---

# Mounting an image as a filesystem

Last week, we had "Impossible Stuff Day" at the Recurse Center. It's a bit like
a one-day hackathon, with the main goal of trying to do something that's
definitely past the [edge of one's abilities][edge], and maybe adjust that edge
afterwards.

I set out to access an image as a filesystem. I'd seen similar projects:

- [TabFS] (mount browser tabs as a filesystem)
- [rssnix] (filesystem-based feed reader)
- [ffs], the file filesystem (mount semi-structured data as a filesystem)

So it was obviously possible. The impossible (to me) aspect was a combination
of having a very fuzzy (at best) idea of where this would be implemented
(something something system calls), and having properly worked with C more than
ten years ago.

[edge]: <https://www.recurse.com/self-directives#work-at-the-edge>
[tabfs]: <https://omar.website/tabfs/>
[rssnix]: <https://github.com/jafarlihi/rssnix>
[ffs]: <https://mgree.github.io/ffs/>

## Research

I hit the library so I could leaf through thick books instead of turning my
mouse wheel. After some quality time with [a][kerrisk] [few][stevens]
[classics][kernighan], I had a very rough idea: there is an abstraction for
common filesystem operations, the *virtual file system*[^1] (VFS), and a
filesystem has to implement that interface. *The Linux Programming Interface*
had a helpful diagram, roughly like this:

[^1]: Also, is it "filesystem" or "file system"? Authors don't agree with each
other, and I've changed it about three times already.

```{.dot .include-source}
graph {
  node [shape=box]
  vfs [label="Virtual File System (VFS)"]
  Application -- vfs
  vfs -- {ext2, ext3, Reiserfs, VFAT, NFS}
}
```

However, specifics about *what* exactly makes up the VFS were a bit more
difficult to come by. The Wikipedia page about VFS and a hint from a [fellow
Recurser][leo] eventually led me to [FUSE], "Filesystem in Userspace".

> **Filesystem in Userspace** (**FUSE**) is a software interface [...] that
> lets non-privileged users create their own file systems without editing
> kernel code.

 Phew :sweat_smile: No kernel hacking for me![^2]

[^2]: See? "Filesystem" and "file system", *in the same sentence* :unamused:

[kerrisk]: <https://man7.org/tlpi/>
[stevens]: <https://www.oreilly.com/library/view/advanced-programming-in/9780321638014/>
[kernighan]: <https://www.pearson.com/en-us/subject-catalog/p/unix-programming-environment-the/P200000000349/9780139376818>
[leo]: <https://leoshimo.com/>
[fuse]: <https://en.wikipedia.org/wiki/Filesystem_in_Userspace>

## FUSE implementations

There is a [reference implementation][libfuse] of FUSE, in the form of a chunky
C library. Apparently, there's sometimes a bit of confusion around FUSE the
interface and libfuse the implementation, with people referring to both as just
"FUSE"---but the important part is that maybe there's another implementation
that is more Benjamin-friendly!

At first, I found [Go bindings for libfuse][go-fuse], which already looked
better to my [Gopher] eyes. And a little later, a from-scratch implementation
in pure Go: [bazil.org/fuse][bazilfuse], via [this blog post][blog] describing
[`zipfs`], which mounts a ZIP archive as a filesystem. I started digging into
the code.

And this is how far I got on the day itself, so it really was impossible for
me! A few people demoed their most excellent projects: a web server in Zig as a
first Zig project; beginnings of a collaborative editor with video chat; a
[font that converts hex colours to RGB][hexagone]; building a neural network;
coming up with an image format from scratch; teaching the [Flipper
Zero][flipper] to understand more types of NFC tags...

[libfuse]: <https://github.com/libfuse/libfuse>
[go-fuse]: <https://github.com/hanwen/go-fuse>
[gopher]: <https://go.dev/blog/gopher>
[bazilfuse]: <https://github.com/bazil/fuse>
[blog]: <https://blog.gopheracademy.com/advent-2014/fuse-zipfs/>
[`zipfs`]: <https://github.com/bazil/zipfs>
[hexagone]: <https://eieio.games/nonsense/hexagone-converting-hex-to-rgb-with-a-font/>
[flipper]: <https://flipperzero.one/>
