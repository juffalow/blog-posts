---
title: 'Google Code Jam 2017: Tidy Numbers'
media_order: google-code-jam.jpg
date: '29-06-2017 19:23'
taxonomy:
    tag:
        - Algorithms
        - 'Google Code Jam'
        - C++
header_image_file: google-code-jam.jpg
header_image_width: 128
header_image_height: 72
---

Tatiana likes to keep things tidy. Her toys are sorted from smallest to largest, her pencils are sorted from shortest to longest and her computers from oldest to newest. One day, when practicing her counting skills, she noticed that some integers, when written in base 10 with no leading zeroes, have their digits sorted in non-decreasing order. Some examples of this are 8, 123, 555, and 224488. She decided to call these numbers tidy. Numbers that do not have this property, like 20, 321, 495 and 999990, are not tidy.

She just finished counting all positive integers in ascending order from 1 to N. What was the last tidy number she counted?

#### Input

The first line of the input gives the number of test cases, T. T lines follow. Each line describes a test case with a single integer N, the last number counted by Tatiana.

#### Output

For each test case, output one line containing Case #x: y, where x is the test case number (starting from 1) and y is the last tidy number counted by Tatiana.

#### Limits

1 ≤ T ≤ 100.

Small dataset : 1 ≤ N ≤ 1000.

Large dataset : 1 ≤ N ≤ 10<sup>18</sup>.

#### Sample

```
Input

4
132
1000
7
111111111111111110

Output

Case #1: 129
Case #2: 999
Case #3: 7
Case #4: 99999999999999999
```

## Solution in C++

It is not too hard to solve this problem after you know how to check if the number is in non-decreasing order and you are working with *small dataset*. You will take the number, test if it is in non-decreasing order. If not, decrease it by 1 and repeat the process until the number is in non-decreasing order.

What is the problem here? If you get a big number, this process will take too long time ( and you have to use proper variable type that can handle big numbers, of course ). So how to optimize it?

#### Decreasing the number

Let’s take the number *132* from the sample input. It’s clearly not in non-decreasing order. If you would follow the steps above, you decrease it by 1 and get 131, then 130 and finally 129 which is tidy. It would be better to extract the last number ( because it violates the order ), decrease the whole number by the last number + 1 and chek again, if it is in the right order. What about the other numbers? Well, if you don’t want to search for another numbers that violates the order and I don’t know what else, you can do this :

1. take 10 as subtrahend
2. is the number in non-decreasing order?
3. if yes, return the number, if no, continue
4. if the number ends with 99 ( i. e. subtrahend * 10 – 1 ), multiply subtrahend by 10
5. substract the subtrahend from the number
6. go to step 2

By how much did we improve the algorithm now? Using the first algorithm, it would take more than 10<sup>16</sup> rounds to get the right result. After this optimization, it takes only 16 rounds!

#### Source code

```
#include 

using namespace std;

bool isNonDecreasing(unsigned long long number) {
    int remainder = 10;
    while( number > 0 ) {
        if( number % 10 <= remainder ) {
            remainder = number % 10;
            number /= 10;
        } else {
            return false;
        }
    }
    return true;
}

unsigned long long lastTidyNumber(unsigned long long number) {
    unsigned long long subtrahend = ( number % 10 ) + 1;
    number -= subtrahend;
    subtrahend = 10;
    while( !isNonDecreasing(number) ) {
        if( number % (subtrahend * 10) == (subtrahend * 10) - 1 ) {
            subtrahend *= 10;
        }
        number -= subtrahend;
    }
    return number;
}

int main() {
    int t;
    cin >> t;
    unsigned long long n;

    for( int i = 0; i < t; i++ ) {
        cin >> n;
        if( isNonDecreasing(n) ) {
            cout << "Case #" << i + 1 << ": " << n << "\n";
        } else {
            cout << "Case #" << i + 1 << ": " << lastTidyNumber(n) << "\n";
        }
    }

    return 0;
}
```

#### Compile and run the code

Compile the code with :

```
$ g++ solution.cpp -o solution
```

Download the small ( *small-input.txt* ) and large ( *large-input.txt* ) input and run the program with command :

```
$ ./solution < small-input.txt > small-output.txt

# or

$ ./solution < large-input.txt > large-output.txt
```

## Conclusion

I hope I explained it well. If you have som other solution, other way how to solve it, please share it in the comments below.

#### References

[Google Code Jam](https://code.google.com/codejam/) official page

[Problem B. Tidy Numbers](https://code.google.com/codejam/contest/3264486/dashboard#s=p1)
