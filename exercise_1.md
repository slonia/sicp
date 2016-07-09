Exercise 1
================
* [1.1](#11)
* [1.2](#12)
* [1.3](#13)
* [1.4](#14)
* [1.5](#15)
* [1.6](#16)
* [1.7](#17)
* [1.8](#18)
* [1.9](#19)
* [1.10](#110)
* [1.11](#111)
* [1.12](#112)
* [1.13](#113)
* [1.14](#114)
* [1.15](#115)
* [1.16](#116)
* [1.17](#117)
* [1.18](#118)
* [1.19](#119)

##1.1

```
10
12
8
3
6
a = 3
b = 4
19
false
4
16
6
16
```

##1.2
```
(/ (+ 5 (+ 4 (- 2 (- 3 (+ 6 (/ 4 5))))))  (* 3 (- 6 2) (- 2 7))) = -37/150
```

##1.3
```
(defn square [x] (* x x))

(defn sumofsquares [x y]
  (+ (square x) (square y))
)

(defn maxofthree [x y z]
  (cond
    (and (< x y) (< x z)) (sumofsquares z y)
    (and (< y x) (< y z)) (sumofsquares x z)
    (and (< z y) (< z x)) (sumofsquares x y)
    )
)
```

##1.4

Functions and predefined constructions in LISP can return other functions and operators. So here IF returns operaton (- or +) and then this operator is applied to function arguments

##1.5

When we need to evaluate argument the program hangs. So in applicative order we will have an error, in normal order the expression evaluates as 0.

##1.6
```
(defn abs [x]
  (if (< x 0) (- x) x)
)

(defn good-enough? [guess x]
  (< (abs (- (* guess guess) x)) 0.001)
)

(defn average [x y]
  (/ (+ x y) 2)
)

(defn improve [guess x]
  (average guess (/ x guess))
)

(defn sqrt-iter [guess x]
  (if (good-enough? guess x)
    guess
    (sqrt-iter (improve guess x) x)
  )
)

(defn sqrt [x]
  (sqrt-iter 1.0 x)
)

(defn new-if [predicate then-clause else-clause]
  (cond predicate then-clause
  :else else-clause)
)

(defn sqrt-iter2 [guess x]
  (new-if (good-enough? guess x)
    guess
    (sqrt-iter (improve guess x) x)
  )
)

(defn sqrt2 [x]
  (sqrt-iter 1.0 x)
)

```

[Scheme version](https://repl.it/C3bk/0)


##1.7
Bad results:
```
(sqrt 0.0004)
```

```
(defn abs [x]
  (if (< x 0) (- x) x)
)

(defn good-enough? [guess prev-guess]
  (< (/ (abs (- guess prev-guess)) guess) 0.001)
)

(defn average [x y]
  (/ (+ x y) 2)
)

(defn improve [guess x]
  (average guess (/ x guess))
)

(defn sqrt-iter [guess prev-guess x]
  (if (good-enough? guess prev-guess)
    guess
    (sqrt-iter (improve guess x) guess x)
  )
)

(defn sqrt [x]
  (sqrt-iter 1.0 0.01 x)
)


```

##1.8

```
(defn abs [x]
  (if (< x 0) (- x) x)
)

(defn good-enough? [guess prev-guess]
  (< (/ (abs (- guess prev-guess)) guess) 0.001)
)

(defn average [x y]
  (/ (+ x y) 2)
)

(defn improve [guess x]
  (/ (+ (* 2 guess) (/ x (* guess guess)))3)
)

(defn cubeqrt-iter [guess prev-guess x]
  (if (good-enough? guess prev-guess)
    guess
    (sqrt-iter (improve guess x) guess x)
  )
)

(defn cubeqrt [x]
  (cubeqrt-iter 1.0 0.01 x)
)


```

##1.9

```
(define (+ a b)
  (if (= a 0)
  b
  (inc (+ (dec a) b))))

```

* (+ 4 5)
* (inc (+ 3 5))
* (inc (inc (+ 2 5)))
* (inc (inc (inc (+ 1 5))))
* (inc (inc (inc (inc (+ 0 5)))))
* (inc (inc (inc (inc 5))))
* (inc (inc (inc 6)))
* (inc (inc 7))
* (inc 8)
* 9

Process is *recursive*, because we should keep track of a, b and num of inc.

```
(define (+ a b)
  (if (= a 0)
  b
  (+ (dec a) (inc b))))
```

* (+ 4 5)
* (+ 3 6)
* (+ 2 7)
* (+ 1 8)
* (+ 0 9)
* 9

Process is *iterative* because we need only a and b to capture the whole state.

##1.10
```
(defn A [x y]
  (cond
    (= y 0) 0
    (= x 0) (* 2 y)
    (= y 1) 2
    :else (A (- x 1)
      (A x (- y 1)))))

>> (A 1 10)
=> 1024

>> (A 2 4)
=> 65536

>> (A 3 3)
=> 65536


(defn f [n] (A 0 n))
(defn g [n] (A 1 n))
(defn h [n] (A 2 n))
(defn k [n] (* 5 n n))

>> (map f (range 1 10))
=> (2 4 6 8 10 12 14 16 18)
>> (map g (range 1 10))
=> (2 4 8 16 32 64 128 256 512)
>> (map h (range 1 5))
=> (2 4 16 65536)

```
BTW: Seems this is not real Ackermann's function.

So this functions are:

* 2*n
* 2^n
* 2^2^2^... n times
* 5*(n^2)

##1.11

Recursive procedure:
```
(defn f-rec [n]
  (cond
    (< n 3) n
    :else (+ (f-rec (- n 1)) (* 2 (f-rec (- n 2))) (* 3 (f-rec (- n 3))))
  )
)
```

Iterative procedure:
```
(defn f-iter [a b c count]
  (if (= count 0)
    a
    (f-iter b c (+ c (* 2 b) (* 3 a)) (dec count))
  )
)

(defn f-it [n]
  (f-iter 0 1 2 n)
)

```

##1.12
```
(defn pasc [x y]
  (if (or (< y 2) (>= y x))
    1
    (+ (pasc (dec x) (dec y)) (pasc (dec x) y))
    )
)
```

##1.13

Lets explore fib(n-2) + fib(n-1)
```
fib(n-2) = (ph^(n-2) - f^(n-2))/sqrt(5)
fib(n-1) = (ph^(n-1) - f^(n-1))/sqrt(5)
fib(n-2) + fib(n-1) = (ph^(n-2) - f^(n-2) + ph^(n-1) - f^(n-1))/sqrt(5) =
= ((ph+1)*ph^(n-2) - (f+1)*f*(n-2))/sqrt(5) = ...
```
In the chapter we see that
```
ph^2 = ph+1
f^2 = f+1
```
so perfroming this substitution
```
... = (ph^2*ph(n-2) - f^2*f(n-2))/sqrt(5) =
= (ph^n - f^n)/sqrt(5) = fib(n)
```
So we prooved that
```
fib(n) = fib(n-2) + fib(n-1)
```

##1.14

```
(defn first-denomination [kinds]
  (cond
    (= kinds 1) 1
    (= kinds 2) 5
    (= kinds 3) 10
    (= kinds 4) 25
    (= kinds 5) 50
  )
)

(defn cc [amount kinds]
  (cond
    (= amount 0) 1
    (or (< amount 0) (= kinds 0)) 0
    :else (+ (cc amount (- kinds 1))
             (cc (- amount (first-denomination kinds)) kinds))
  )
)

(defn count-change [amount]
  (cc amount 5)
)

```

![Tree](images/tree.png)

We have 5 steps until we get (cc (- amount 4) 1) and after that we have (- amount 4) steps. Maximum width of the is 5 so order of growth for number of steps is *theta(amount^5)*. Height of the tree is proportional for amount, so the order of growth for memory is *theta(amount)*.

##1.15
```
(defn abs [x]
  (if (< x 0) (- x) x)
)
(defn cube [x]
 (* x x x)
)
(defn p [x]
  (- (* 3 x) (* 4 (cube x))))
)
(defn sine [angle]
  (if (not (> (abs angle) 0.1))
  angle
  (p (sine (/ angle 3.0))))
)

```
a) *p* will be called 5 times:
* 12.15
* 4.05
* 1.35
* 0.45
* 0.15
* 0.05

We can see that after 5th iteration value is < 0.1

b) Number of iterations is log(3, 10 * a), which is *theta(log(a))*:
n - number of iterations, so

```
a/(3^n) < 0.1
a < 0.1 * (3^n)
10a < 3^n
log(3, 10*a) < n
```

On each step we should store constant value of variables, so the order of memory growth is the same.

##1.16

Recursive procedure

```
(defn even? [n]
(= (rem n 2) 0))

(defn square [x] (* x x))

(defn fast-expt [b n]
  (cond
    (= n 0) 1
    (even? n) (square (fast-expt b (/ n 2)))
    :else (* b (fast-expt b (dec n)))
  )
)
```

Iterative procedure
```
(defn fast-expt-iter [a b n]
  (cond
    (= n 1) a
    (even? n) (fast-expt-iter (* a (square b)) b (/ n 2))
    :else (fast-expt-iter (* a b) b (dec n))
  )
)

(defn fast-expt2 [b n]
  (fast-expt-iter 1 b n)
)
```

##1.17
```
(defn even? [n]
(= (rem n 2) 0))

(defn dbl [x]
  (+ x x)
)

(defn halve [x]
  (/ x 2)
)

(defn fast-mult [a b]
  (cond
    (or (= a 0) (= b 0)) 0
    (= a 1) b
    (= b 1) a
    (even? b) (fast-mult (dbl a) (halve b))
    :else (+ a (fast-mult a (dec b)))
  )
)
```

##1.18

```
(defn even? [n]
(= (rem n 2) 0))

(defn dbl [x]
  (+ x x)
)

(defn halve [x]
  (/ x 2)
)

(defn fast-mult-iter [r a b]
  (cond

    (= b 1) r
    (even? b) (fast-mult-iter (+ r (dbl a)) a (halve b))
    :else (fast-mult-iter (+ r a) a (dec b))
  )
)

(defn fast-mult2 [a b]
  (fast-mult-iter 0 a b)
)

```

##1.19

Lets count a<sub>2</sub> and b<sub>2</sub> in terms of *p* and *q*:
<pre>
a<sub>1</sub> = b<sub>0</sub>q + a<sub>0</sub>(q+p)
b<sub>1</sub> = b<sub>0</sub>p + a<sub>0</sub>q

a<sub>2</sub> = b<sub>1</sub>q + a<sub>1</sub>(q+p)
b<sub>2</sub> = b<sub>1</sub>p + a<sub>1</sub>q
</pre>

Perfom substitution of the first equations into the second:

<pre>
a<sub>2</sub> = q<sup>2</sup>(2a<sub>0</sub> + b<sub>0</sub>) + p<sup>2</sup>a<sub>0</sub> + pq(2a<sub>0</sub> + 2b<sub>0</sub>)
b<sub>2</sub> = q<sup>2</sup>(a<sub>0</sub> + b<sub>0</sub>) + p<sup>2</sup>b<sub>0</sub> + 2a<sub>0</sub>pq
</pre>

Lets use that a<sub>0</sub> = 1 and b<sub>0</sub> = 0:
<pre>
a<sub>2</sub> = 2q<sup>2</sup> + p<sup>2</sup> + 2pq
b<sub>2</sub> = q<sup>2</sup> + 2pq
</pre>

Now lets count a<sub>2</sub> and b<sub>2</sub> in terms of *p`* and *q`*:
<pre>
a<sub>2</sub> = b<sub>0</sub>q` + a<sub>0</sub>(q`+p`)
b<sub>2</sub> = b<sub>0</sub>p` + a<sub>0</sub>q`

a<sub>2</sub> = q`+p`
b<sub>2</sub> = q`
</pre>

From first and second systems we get
<pre>
q` = q<sup>2</sup> + 2pq
p` = q<sup>2</sup> + p<sup>2</sup>
</pre>

Now we can use this in fib-iter function:
```
(defn even? [n]
  (= (rem n 2) 0))


(defn fib-iter [a b p q count]
  (cond
    (= count 0) b
    (even? count)
      (fib-iter a b
        (+ (* q q) (* p p))
        (+ (* q q) (* 2 p q))
        (/ count 2)
        )
    :else (fib-iter
      (+ (* b q) (* a q) (* a p))
      (+ (* b p) (* a q))
      p
      q
      (dec count)
    )
  )
)


(defn fib [n]
  (fib-iter 1 0 0 1 n)
)
```
