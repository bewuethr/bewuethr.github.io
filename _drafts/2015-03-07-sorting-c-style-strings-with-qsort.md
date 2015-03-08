---
published: false
---

## Sorting C-style strings with qsort

While working on one of the last exercises in Stroustrup's [Programming &ndash; Principle and Practice Using C++](http://www.stroustrup.com/Programming/PPP1.html), I came across this little problem: assume you want to sort an array of C-style strings using `qsort()` from the C standard library (`<cstdlib>` in C++ or `<stdlib.h>` in C).

<!---more--->

An array of C-style strings looks something like `char* words[ARR_LEN]`, where `ARR_LEN` is the number of strings in your array. `qsort` is declared as follows:

```C
void qsort(void* ptr, size_t count, size_t size,
           int (*comp)(const void*, const void*));
```

`ptr` points to your array, `count` is the number of elements in the array, `size` the size of an element and `comp` a comparison function that returns

* a _negative_ value if the first argument is smaller than the second,
* a _positive_ value if it is bigger than the second, and
* _zero_ if they are equal.

Like `strcmp`, if you will.

My strings all have the same length, `WORD_LEN`. Obviously, I try to call qsort like this:

```C
qsort(words,ctr,WORD_LEN,strcmp);
```

where `ctr` is the number of strings in `words`, and I want to use `strcmp` as my comparison function. This compiles just fine, but after the call to `qsort`, things are less fine: "stack corrupted". Hmmm. I start googling.

