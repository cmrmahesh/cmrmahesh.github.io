---
title: Neutralisation of Stars
tags: c++, cp, hackerearth, stack
category:
  - code
---

Q: Each char will have charge and same charges nullify/neutralize. Print the final string where no more neutralisation can happen.

Ex:
aacccd -> cd
aaacccbbcccd -> accd -> ad

Learnings:

1. If trying to do some removal operation on an array in `O(n)` think stack.

BruteForce:

```c++
#include <bits/stdc++.h>
using namespace std;

int main()
{
    int n;
    string s;
    cin >> n;
    cin >> s;
    string temp, output;
    int prevlen, curlen;
    prevlen = -1;
    curlen = n;
    temp = s;
    while (1)
    {
        prevlen = temp.length();
        for (int i = 0; i < curlen;)
        {
            if (temp[i] != temp[i + 1])
            {
                output += temp[i];
                i++;
            }
            else if (temp[i] == temp[i + 1])
            {
                i = i + 2;
            }
        }
        curlen = output.length();
        temp = output;
        if (prevlen == curlen)
        {
            break;
        }
        output = "";
    }
    cout << output.length() << endl;
    cout << output << endl;
}
```

This is the initial implementation. Worst kind even for brute force time complexity `O(n<sup>2<\sup>)` and space complexity `O(n)`.

And the best solution I found is:

<iframe src=https://www.hackerearth.com/submission/key/66b72a24961c4fc8b77da240b24497cd/?theme=light&content-length=1032 width='100%' height='1574px' frameborder='0' allowtransparency='true' scrolling='yes'></iframe>
