---
title: 'Lexicographical permutations'
media_order: hackerrank.png
taxonomy:
    tag:
        - Algorithms
        - HackerRank
        - C++
header_image_file: hackerrank.png
header_image_width: 128
header_image_height: 128
---

The solution for Bigger is Greater challenge on HackerRank, or how to generate next lexicographically greater string. Problem description, algorithm steps and complete source code to solve this challenge.

===

From time to time I am solving “challenges” in my free time on [Project Euler](https://projecteuler.net/) or [HackerRank](https://www.hackerrank.com/). This time it was the challenge [Bigger is Greater](https://www.hackerrank.com/challenges/bigger-is-greater/problem) on HackerRank. It’s about finding next lexicographically greater string than the given string.

## Problem

Given a word w, rearrange the letters of *w* to construct another word s in such a way that s is lexicographically greater than *w*. In case of multiple possible answers, find the lexicographically smallest one among them.

#### Input

The first line of input contains *t*, the number of test cases. Each of the next *t* lines contains *w*.

```
5
ab
bb
hefg
dhck
dkhc
```

#### Output

For each testcase, output a string lexicographically bigger than *w* in a separate line. In case of multiple possible answers, print the lexicographically smallest one, and if no answer exists, print *no answer*.

```
ba
no answer
hegf
dhkc
hcdk
```

## Solution in C++

#### Steps

Here are the steps / algorithm to generate the next higher permutation :

1. take the string and find highest index `i` that `string[i] < string[i + 1]`
2. find the highest index `j` > `i` such that `string[j]` > `string[i]`
3. swap `string[i]` with `string[j]`
4. reverse the order of all elements after index `i`

#### Source code

```
#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>
#include <string.h>
using namespace std;

int compare (const void *a, const void * b) {
    return ( *(char *)a - *(char *)b );
}

void swap (char* a, char* b) {
    char t = *a;
    *a = *b;
    *b = t;
}

int findCeil (char str[], char first, int l, int h) {
    int ceilIndex = l;

    for( int i = l+1; i <= h; i++ ) {
        if( str[i] > first && str[i] < str[ceilIndex] ) {
            ceilIndex = i;
        }
    }

    return ceilIndex;
}

bool nextPermutation(char* str) {
    int size = strlen(str);

    int i;
    for ( i = size - 2; i >= 0; --i ) {
        if( str[i] < str[i + 1] ) {
            break;
        }
    }

    if( i == -1 ) {
        return false;
    } else {
        int ceilIndex = findCeil( str, str[i], i + 1, size - 1 );

        swap( &str[i], &str[ceilIndex] );

        qsort( str + i + 1, size - i - 1, sizeof(str[0]), compare );
    }
    return true;
}

int main() {
    int tests;
    cin >> tests;

    string str;
    for( int i = 0; i < tests; i++ ) {
        cin >> str;
        char *cstr = new char[str.length() + 1];
        strcpy(cstr, str.c_str());
        if( nextPermutation(cstr) ) {
            cout << cstr << "\n";
        } else {
            cout << "no answer\n";
        }
    }

    return 0;
}
```

#### Source code #2

There is a *Need help?* section on the right side of the HackerRank challenge and it recommends *Next permutation* topic. You can read that in most languages there is the *next permutation* function already implemented. In C++ it is `next_permutation`.

```
#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>
#include <string.h>
using namespace std;

int main() {
    int tests;
    cin >> tests;

    string str;
    for( int i = 0; i < tests; i++ ) {
        cin >> str;
        if( next_permutation(str.begin(), str.end()) != 0 ) {
            cout << str << "\n";
        } else {
            cout << "no answer" << "\n";
        }
    }

    return 0;
}
```

## Conclusion

These are two working examples how you can solve the problem above. You can read more about lexicographic permutations on pages below in References section.

#### References

[GeeksforGeeks - Print all permutations in sorted ( lexicographic ) order](http://www.geeksforgeeks.org/lexicographic-permutations-of-string/)

[Wikipedia - Leicographical order](https://en.wikipedia.org/wiki/Lexicographical_order)
