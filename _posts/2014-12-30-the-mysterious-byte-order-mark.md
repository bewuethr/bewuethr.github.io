---
layout: post
title: The mysterious byte order mark
published: true
---

I bumped into a strange problem with reading a text file recently. The file described the layout of a [Nonogram](http://en.wikipedia.org/wiki/Nonogram) for a program my father was working on:

```
N 5 5
Z 5 0 1 0 2 1 0 1 0 1 0
K 3 0 1 1 0 1 0 1 1 0 1 2 0
```

And to read the first line, he used something like this:

```cpp
char ch;
int i1;
int i2;
ifstream ifs("Goobix 5.nono");
ifs >> ch >> i1 >> i2;
```

However, this didn't work, the `int`s didn't get read successfully. I then tried

```cpp
char ch;
ifstream ifs("Goobix 5.nono");
while (ifs.get(ch))
	cout << ch;
```

to not ignore whitespace, resulting in this:

![2014_12_30-01.png](/images/2014_12_30-01.png)

Behold, three unwanted characters! What are they? Let's cast them to `int` to find out more:

```cpp
char ch;
ifstream ifs("Goobix 5.nono");
while (ifs.get(ch))
	cout << ch << " (int: " << int(ch) << ")\n";
```

and we get

![2014_12_30-02.png](/images/2014_12_30-02.png)

Negative, eh? Strange. [Other people have seen similar things](http://stackoverflow.com/questions/4690415/negative-ascii-value), so apparently these aren't normal ASCII characters. To find out more, I've looked at the text file with a hex editor:

![2014_12_30-03.png](/images/2014_12_30-03.png)

The file starts with `0xef 0xbb 0xbf`. That's googleable! and leads to [byte order marks](http://en.wikipedia.org/wiki/Byte_order_mark) (BOM). The BOM indicates endianness and encoding of the text file; in the case of `0xef 0xbb 0xbf`, it is a UTF-8 encoded file. To get rid of the BOM, we can just save the file with ANSI encoding:

![2014_12_30-04.png](/images/2014_12_30-04.png)

Now, the file behaves as expected when being read:

![2014_12_30-05.png](/images/2014_12_30-05.png)

Encodings in C++ (and elsewhere!) can be daunting. Good readings I have found include these:

* [The Absolute Minimum Every Software Developer Absolutely, Positively Must Know About Unicode and Character Sets (No Excuses!)])(http://www.joelonsoftware.com/articles/Unicode.html) by Joel Spolsky
* [Character Encodings For Modern Programmers](http://blog.gatunka.com/2014/04/25/character-encodings-for-modern-programmers/) by GT
* And, one day, should I have lots of spare time: [Standard C++ IOStreams and Locales](http://amzn.com/0201183951) by Angelika Langer and Klaus Kreft
