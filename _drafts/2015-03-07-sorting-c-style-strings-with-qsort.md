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

where `ctr` is the number of strings in `words`, and I want to use `strcmp` as my comparison function. This compiles just fine, but after the call to `qsort`, things are less fine: "stack corrupted". Hmmm.

I realize that the size of an element in my array is not the size of each individual string, but rather the size of a `char*`, because that's what the array is: an array of `char*`, and not an array of actual strings. I change my call to

```C
qsort(words,ctr,sizeof(char*),strcmp);
```

Still compiles without problems. Good. And doesn't crash. Even better! However, the results are still not quite what I want. If start with

```C
char* words[ARR_LEN] = { "bb", "cc", "aa" };
```

and sort that, I get "cc aa bb" as the result.

I'm pretty sure I'm not the first person ever to try and sort C-strings with `qsort`, so I google. I come across [this Stack Overflow question](http://stackoverflow.com/questions/3757899/sorting-strings-using-qsort) where somebody had a very similar problem. The problem is that the `comp` argument of `qsort` expects `const void*` arguments whereas `strcmp` expects `const char*`. The solution is to wrap the call to `strcmp` into a function that performs the proper casts, like so:

```C
int cmpstr(const void* a, const void* b) { 
    const char* aa = (const char*)a;
    const char* bb = (const char*)b;
    return strcmp(aa, bb);
}
```

This takes `const void*`, like required by `qsort`'s `comp`, and casts them to `const char*`, like required by `strcmp`. Neat. I call `qsort` with my new function:

```C
qsort(words,ctr,sizeof(char*),cmpstr);
```





