Exercise 1
================

**1.1**
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

**1.2**
```
(/ (+ 5 (+ 4 (- 2 (- 3 (+ 6 (/ 4 5))))))  (* 3 (- 6 2) (- 2 7))) = -37/150
```

**1.3**
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

**1.4**

Functions and predefined constructions in LISP can return other functions and operators. So here IF returns operaton (- or +) and then this operator is applied to function arguments

**1.5**

When we need to evaluate argument the program hangs. So in applicative order we will have an error, in normal order the expression evaluates as 0.

**1.6**
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


**1.7**
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

**1.8**

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

