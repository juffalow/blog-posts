---
title: 'Prime numbers'
media_order: algorithm.png
date: '29-03-2017 19:22'
taxonomy:
    tag:
        - Algorithms
        - Go
        - Java
header_image_file: algorithm.png
header_image_width: 128
header_image_height: 128
---

With this post I want to begin a series of posts about general algorithms. It is not an accident I started with the prime numbers. This is basic problem to solve when you are starting with programming. And one more reason – I was curious how RSA algorithm works and the important part of the algorithm is to generate two random big prime numbers.

===

## Basic method

The most simple solution is to iterate through all numbers from 2 to n – 1 and check if n is divisible by this number. This can be optimized when observing that all primes are of the form 6k ± 1, with the exception of 2 and 3.

Why is that so? All numbers can be expressed as (6k + i) for some integer k and for i = -1, 0, 1, 2, 3 or 4. Because prime number must not be even ( other than 2 ), any numbers of the form 6k, 6k + 2, 6k + 4 must be composite. The same it is with 3. Prime number must not be divisible by 3 ( other then 3 ) and 6k + 3 = 3(2k + 1), which is divisible by 3. This leaves only 6k + 1 and 6k – 1.

And instead of checking until n, check only till √n beacause larger factor of n must be a multiple of smaller factor that has been already checked.

#### Java

```
boolean isPrime(int n) {
    // corner cases
    if( n <= 1 ) { return false; }
    if( n <= 3 ) { return true; }

    // this checks 6k, 6k + 2, 6k + 4 and 6k + 3
    if( n % 2 == 0 || n % 3 == 0 ) { return false; }

    for( int i = 5; i * i <= n; i = i + 6 ) {
        // it starts with number 5, which is 6k - 1
        // and test n % ( i + 2 ) means 6k + 1
        if( n % i == 0 || n % (i + 2) == 0 ) {
            return false;
        }
    }
    return true;
}
```

#### Go

```
func isPrime(n int) bool {
    // corner cases
    if n <= 1 {
        return false
    }

    if n <= 3 {
        return true
    }

    // this checks 6k, 6k + 2, 6k + 4 and 6k + 3
    if n % 2 == 0 || n % 3 == 0 {
        return false
    }

    for i := 5; i * i <= n; i = i + 6 {
        // it starts with number 5, which is 6k - 1
        // and test n % ( i + 2 ) means 6k + 1
        if n % i == 0 || n % (i + 2) == 0 {
            return false
        }
    }
    return true
}
```

## Fermat method

> Tout nombre premier mesure infailliblement une des puissances – 1 de quelque progression que ce soit, et l'exposant de la dite puissance est sous-multiple du nombre premier donné – 1 ; et, après qu'on a trouvé la première puissance qui satisfait à la question, toutes celles dont les exposants sont multiples de l'exposant de la première satisfont tout de même à la question.
> - [Pierre de Fermat](https://en.wikipedia.org/wiki/Pierre_de_Fermat)

Simpler formulation : If p is a prime and a is any integer not divisible by p, then a<sup>p - 1</sup> - 1 is divisible by p.

The problem with Fermat's test is that it's not completely reliable. If a given number is prime, then this method always returns true. If given number is composite, then it may return true or false.

<div class="alert alert-info" role="alert">There are many numbers, for which all values of a &lt; n satisfies the condition. These numbers are known as <a href="https://en.wikipedia.org/wiki/Carmichael_number">Carmichael numbers</a> and they are substantially rarer than prime numbers.</div>

#### Java

```
boolean isPrime(int n) {
    int x;

    if (n <= 1 || n == 4)  return false;
    if (n <= 3) return true;

    for( int i = 0; i < 10; i++ ) {
        x = 1 + (int)(Math.random() * (n - 1));

        if ( moduloPower(x, n - 1, n) != 1 ) {
            return false;
        }
    }

    return true;
}
/**
 * Calculate (base ^ exponent) % modulus
 * Uses Modular Exponentation, or better exponentation by squaring
 * ( also known as binary exponentation )
 */
int moduloPower(int base, int exponent, int modulus) {
    int result = 1;
    base = base % modulus;

    while( exponent > 0 ) {
        if((exponent & 1) == 1) {
            result = (result * base) % modulus;
        }

        exponent = exponent >> 1;
        base = (base * base) % modulus;
    }
    return result;
}
```

#### Go

```
func isPrime(n int) bool {
    var x int

    if n <= 1 || n == 4 {
        return false
    }
    if n <= 3 {
        return true
    }

    rand.Seed(time.Now().Unix())

    for i := 0; i < 10; i++ {
        x = 1 + rand.Intn(n - 1)

        if moduloPower(x, n - 1, n) != 1 {
            return false
        }
    }

    return true
}

func moduloPower(base int, exponent int, modulus int) int {
    result := 1
    base = base % modulus

    for exponent > 0  {
        if (exponent & 1) == 1 {
            result = (result * base) % modulus
        }

        exponent = exponent >> 1
        base = (base * base) % modulus
    }
    return result
}
```

## Sieve of Eratosthenes

Simple algorithm for finding all prime numbers up to any given limit. It does the job by marking all multiples of each unmarked number in order. That means you take number 2 and mark all its multiples. Then you find next unmarked number, which is 3 and again, mark all its multiples... Example :

Mark all multiples of 2 :

<table class="table table-bordered">
    <tbody>
    	<tr>
    		<td></td>
    		<td>2</td>
    		<td>3</td>
    		<td class="danger">4</td>
    		<td>5</td>
    		<td class="danger">6</td>
    		<td>7</td>
    		<td class="danger">8</td>
    		<td>9</td>
    		<td class="danger">10</td>
    	</tr>
    	<tr>
    		<td>11</td>
    		<td class="danger">12</td>
    		<td>13</td>
    		<td class="danger">14</td>
    		<td>15</td>
    		<td class="danger">16</td>
    		<td>17</td>
    		<td class="danger">18</td>
    		<td>19</td>
        	<td class="danger">20</td>
    	</tr>
    	<tr>
    		<td>21</td>
    		<td class="danger">22</td>
    		<td>23</td>
    		<td class="danger">24</td>
    		<td>25</td>
    		<td class="danger">26</td>
    		<td>27</td>
    		<td class="danger">28</td>
    		<td>29</td>
    		<td class="danger">30</td>
    	</tr>
    </tbody>
</table>

Mark all multiples of 3 :

<table class="table table-bordered">
<tbody><tr>
<td></td>
<td>2</td>
<td>3</td>
<td class="danger">4</td>
<td>5</td>
<td class="danger">6</td>
<td>7</td>
<td class="danger">8</td>
<td class="warning">9</td>
<td class="danger">10</td>
</tr>
<tr>
<td>11</td>
<td class="danger">12</td>
<td>13</td>
<td class="danger">14</td>
<td class="warning">15</td>
<td class="danger">16</td>
<td>17</td>
<td class="danger">18</td>
<td>19</td>
<td class="danger">20</td>
</tr>
<tr>
<td class="warning">21</td>
<td class="danger">22</td>
<td>23</td>
<td class="danger">24</td>
<td>25</td>
<td class="danger">26</td>
<td class="warning">27</td>
<td class="danger">28</td>
<td>29</td>
<td class="danger">30</td>
</tr>
</tbody></table>

Mark all multiples of 5 :

<table class="table table-bordered">
<tbody><tr>
<td></td>
<td>2</td>
<td>3</td>
<td class="danger">4</td>
<td>5</td>
<td class="danger">6</td>
<td>7</td>
<td class="danger">8</td>
<td class="warning">9</td>
<td class="danger">10</td>
</tr>
<tr>
<td>11</td>
<td class="danger">12</td>
<td>13</td>
<td class="danger">14</td>
<td class="warning">15</td>
<td class="danger">16</td>
<td>17</td>
<td class="danger">18</td>
<td>19</td>
<td class="danger">20</td>
</tr>
<tr>
<td class="warning">21</td>
<td class="danger">22</td>
<td>23</td>
<td class="danger">24</td>
<td class="info">25</td>
<td class="danger">26</td>
<td class="warning">27</td>
<td class="danger">28</td>
<td>29</td>
<td class="danger">30</td>
</tr>
</tbody></table>

Now, all unmarked numbers are prime numbers.

#### Java

```
List sieveOfEratosthenes(int n) {
    boolean[] prime = new boolean[n + 1];

    for(int p = 2; p * p <= n; p++) {
        if(prime[p] == false) {
            for(int i = p * 2; i <= n; i+= p) {
                prime[i] = true;
            }
        }
    }

    List sieve = new ArrayList<>();

    for(int i = 2; i <= n; i++) {
        if(prime[i] == false) {
            sieve.add(i);
        }
    }

    return sieve;
}
```

#### Go

```
func sieveOfEratosthenes(n int) []int {
	prime := make([]bool, n + 1)

	for p := 2; p * p <= n; p++ {
		if prime[p] == false {
			for i := p * 2; i <= n; i+= p {
				prime[i] = true
			}
		}
	}

	sieve := make([]int, 0)

	for i := 2; i <= n; i++ {
		if prime[i] == false {
			sieve = append(sieve, i)
		}
	}

	return sieve
}
```

## Conclusion

I am preparing posts about other common algorithms - sorting and searching algorithms, shortest paths, cryptography, etc.

P.S.: I skipped Miller-Rabin algorithm - this one will be added later.

#### References

[Wikipedia - Prime number](https://en.wikipedia.org/wiki/Prime_number)

[GeeksforGeeks - Primality Test | Set 1 (Introduction and School Method)](http://www.geeksforgeeks.org/primality-test-set-1-introduction-and-school-method/)

[GeeksforGeeks - Primality Test | Set 2 (Fermat Method)](http://www.geeksforgeeks.org/primality-test-set-2-fermet-method/)

[Wikipedia - Fermat primality test](https://en.wikipedia.org/wiki/Fermat_primality_test)

[Wikipedia - Fermat's little theorem](https://en.wikipedia.org/wiki/Fermat's_little_theorem)

[Wikipedia - Modular exponentiation](https://en.wikipedia.org/wiki/Modular_exponentiation)

[GeeksforGeeks - Sieve of Eratosthenes](http://www.geeksforgeeks.org/sieve-of-eratosthenes/)

[Wikipedia - Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)
