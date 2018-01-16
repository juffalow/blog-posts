---
title: 'Eight queens problem'
media_order: nqueens.png
date: '05-10-2016 19:19'
taxonomy:
    tag:
        - Algorithms
        - Go
header_image_file: nqueens.png
header_image_width: 128
header_image_height: 131
---

If you know what is Bactracking, you sure know about Eight Queens problem – how to place eight queens on an 8×8 chessboard. I found nice solution using bitwise operations which counts all solutions for any given size.

===

>The eight queens puzzle is the problem of placing eight chess queens on an 8×8 chessboard so that no two queens threaten each other. Thus, a solution requires that no two queens share the same row, column, or diagonal.
> - [Wikipedia](https://en.wikipedia.org/wiki/Eight_queens_puzzle)

The common solution for this is to use backtracking. It is a good practice and you understand the algorithm realy fast. But it still can be improved a little bit…

## Bitwise solution in Go lang

I found this solution with very good explanation on internet. It uses only bitwise operations, therefore it is really fast. Originally, it was written in JavaScript, but I rewrote it in Go.

#### The code

```
package main

import (
    "fmt"
    "math"
)

func main() {
    fmt.Println(countNQueenSolution(4))
}

func countNQueenSolution(n int) int {
    var count int
    count = 0

    var done int
    done = int(math.Pow(2, float64(n))) -1

    var recursion func(ld int, col int, rd int)

    recursion = func(ld int, col int, rd int) {
        if col == done {
            count++
            return
        }

        poss := ^(ld | col | rd)

        for poss & done > 0 {
            bit := poss & -poss
            poss -= bit
            recursion((ld|bit)>>1, col|bit, (rd|bit)<<1)
        }
    }

    recursion(0, 0, 0)

    return count
}
```

#### Explanation

When you see it for the first time, it can be a little bit confusing, but later you find it perfect. I suppose you are familiar that computer stores data as sequence of bits ( 5 = 0101 ). Now look on the code and see the variables `ld`, `col` and `rd`.

* for N = 4, `col` having value of `0010` would mean that the 3rd column is already occupied by a queen
* for N = 8, `ld` having a value of `00011000` at row 5 would mean that the top-left-to-bottom-right diagonals that pass through columns 4 and 5 of that row are already occupied by queens

```
var done int
done = int(math.Pow(2, float64(n))) -1
```

The algorithm will not break the chess rules, that means it doesn't place queen on the same row / column / diagonal with any other queen. So when every place in row is taken - the `col` variable has each of its bits set to `1` - we found one of the solutions. The `done` variable is used for this. If N = 4, we set its value to `2<sup>4</sup> - 1` = `15` = `1111` in binary.

```
if col == done {
    count++
    return
}
```

Here is the condition to check if we found one of the solutions. If each column is occupied, then `col` = `1111` and it corresponds to `done` value ( for N = 4 ).

```
poss := ^(ld | col | rd)
```

In `ld`, `col` and `rd` are the queens positions for current row. When doing OR, we get all under attack positions in that row. Then negate (`^`) it to get free positions.

```
bit := poss & -poss
poss -= bit
```

Get the first available position and mark that position as taken in the current row.

```
recursion((ld|bit)>>1, col|bit, (rd|bit)<<1)
```

Move to next row. So you shift diagonals and mark new position in `col` variable.

## Conclusion

When you have some problem to solve, it is good to try and solve it your way ( if you have time of course ) and then look for another solution(s) so you can compare it and learn something.

#### References

[Eight queens puzzle](https://en.wikipedia.org/wiki/Eight_queens_puzzle) on Wikipedia with problem's history, solutions and related problems.

Original post, [A bitwise solution to the N queens problem in JavaScript](http://gregtrowbridge.com/a-bitwise-solution-to-the-n-queens-problem-in-javascript/), which I found and rewrote.

PDF [Backtracking Algorithms in MCPL using Bit Patterns and Recursion](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.51.7113&rep=rep1&type=pdf) by Martin Richards, where you can find more informations.
