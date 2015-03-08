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

`ptr` points to your array, `count` is the number of elements in the array, `size` the size of an element and `comp` a comparison function that returns a negative value if the first argument is smaller than the second, a positive value if it is bigger than the second and zero if they are equal. Like `strcmp`, if you will.