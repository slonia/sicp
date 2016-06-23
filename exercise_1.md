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

**Coins**
```
(defn count-change [amount]
  (cc amount 5)
)

(defn cc [amount kinds]
  (cond
    (= amount 0) 1
    (or (< amount 0) (= kinds 0)) 0
    :else (+ (cc amount (- kinds 1))
             (cc (- amount (first-denomination kinds)) kinds))
  )
)

(defn first-denomination [kinds]
  (cond
    (= kinds 1) 1
    (= kinds 2) 5
    (= kinds 3) 10
    (= kinds 4) 25
    (= kinds 5) 50
  )
)

```

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

##Eleven

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


