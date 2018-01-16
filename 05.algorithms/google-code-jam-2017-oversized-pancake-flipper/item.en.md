---
title: 'Google Code Jam 2017: Oversized Pancake Flipper'
media_order: google-code-jam.jpg
date: '25-04-2017 19:23'
taxonomy:
    tag:
        - JavaScript
        - Algorithms
        - 'Google Code Jam'
header_image_file: google-code-jam.jpg
---

Last year, the Infinite House of Pancakes introduced a new kind of pancake. It has a happy face made of chocolate chips on one side (the “happy side”), and nothing on the other side (the “blank side”).

You are the head cook on duty. The pancakes are cooked in a single row over a hot surface. As part of its infinite efforts to maximize efficiency, the House has recently given you an oversized pancake flipper that flips exactly *K* consecutive pancakes. That is, in that range of *K* pancakes, it changes every happy-side pancake to a blank-side pancake, and vice versa; it does not change the left-to-right order of those pancakes.

You cannot flip fewer than *K* pancakes at a time with the flipper, even at the ends of the row (since there are raised borders on both sides of the cooking surface). For example, you can flip the first *K* pancakes, but not the first *K* – 1 pancakes.

Your apprentice cook, who is still learning the job, just used the old-fashioned single-pancake flipper to flip some individual pancakes and then ran to the restroom with it, right before the time when customers come to visit the kitchen. You only have the oversized pancake flipper left, and you need to use it quickly to leave all the cooking pancakes happy side up, so that the customers leave feeling happy with their visit.

Given the current state of the pancakes, calculate the minimum number of uses of the oversized pancake flipper needed to leave all pancakes happy side up, or state that there is no way to do it.

#### Input

The first line of the input gives the number of test cases, *T*. *T* test cases follow. Each consists of one line with a string *S* and an integer *K*. *S* represents the row of pancakes: each of its characters is either + (which represents a pancake that is initially happy side up) or – (which represents a pancake that is initially blank side up).

#### Output

For each test case, output one line containing Case #x: y, where x is the test case number (starting from 1) and y is either IMPOSSIBLE if there is no way to get all the pancakes happy side up, or an integer representing the the minimum number of times you will need to use the oversized pancake flipper to do it.

#### Limits

1 ≤ T ≤ 100.

Every character in S is either + or -.

2 ≤ K ≤ length of S.

Small dataset : 2 ≤ length of S ≤ 10.

Large dataset : 2 ≤ length of S ≤ 1000.

#### Sample

```
Input

3
---+-++- 3
+++++ 4
-+-+- 4

Output

Case #1: 3
Case #2: 0
Case #3: IMPOSSIBLE
```

## Solution in JavaScript

When I saw it for the first time I thought it is pretty hard problem. Leter, unfortunately too late, I realized it is not that hard. You have to find first blank side pancake and flip *K* pancakes. And again, find the next blank side pancake and flip. If you find blank side pancake less than *K* pancakes before end, it is impossible to flip all of them. Otherwise print the number of flipps.

#### Source code

I am not saying that JavaScript is the best language for this purpose. Few days before this competition begins I was doing something in JavaScript so I used it.

```
var readline = require('readline');

var readLines = 0;

var testCases;

var rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
  terminal: false
});

rl.on('line', function (line) {
    if( readLines == 0 ) {
        testCases = parseInt(line);
    } else {
        var parts = line.split(' ');
        var pancakes = parts[0];
        var flipperSize = parseInt(parts[1]);
        var boolPancakes = new Array(pancakes.length);
        for( var i = 0; i < pancakes.length; i++ ) {
            boolPancakes[i] = (pancakes[i] == '+');
        }
        solve(boolPancakes, flipperSize, readLines);
    }

    readLines++;
});

function solve(pancakes, flipperSize, caseIndex) {
    var flipps = 0;

    for( var i = 0; i < pancakes.length; i++ ) {
        if( !pancakes[i] ) {
            if( i + flipperSize <= pancakes.length ) {
                for( var j = i; j < i + flipperSize; j++ ) {
                    pancakes[j] = !pancakes[j];
                }
                flipps++;
            } else {
                console.log("Case #" + caseIndex + ": IMPOSSIBLE");
                return;
            }
        }
    }
    console.log("Case #" + caseIndex + ": " + flipps);
}
```

#### Run the code

Download the small ( *small-input.txt* ) and large ( *large-input.txt* ) input and run the code with command :

```
node solution.js < small-input.txt > small-output.txt

# or

node solution.js < large-input.txt > large-output.txt
```

## Conclusion

The source code is not nice, it is a contest and the programs are written quickly and there is no need to maintain them after the contest.

#### References

[Google Code Jam](https://code.google.com/codejam/) official page

[Problem A. Oversized Pancake Flipper](https://code.google.com/codejam/contest/3264486/dashboard)
