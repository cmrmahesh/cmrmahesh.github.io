---
title: Petty annoyances in C++
tags: c++
category:
  - code
---

Having been in python land for nearly 2 years, I am visiting c++ again. WHat I found is too many inconsistences and petty annoyances. I will try to list them down so that I can be corrected.

1. `length` and `size`.

For string in c++

```c++
std::string str="hey";
std::cout << str.length() << std::endl;
std::cout << str.size() << std::endl;
```

Here the output is:

```c++
3
3
```

Both `string::size` and `string::length` are synonyms and return the same value.

In case of a vector there is no length function at all.

```c++
std::vector<int> v(4, 0);
std::cout << v.size() << std::endl;
```

So I think I made my point here. Why vector why?
