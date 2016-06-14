Excercise 1
================

1.1
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

1.2
```
(/ (+ 5 (+ 4 (- 2 (- 3 (+ 6 (/ 4 5))))))  (* 3 (- 6 2) (- 2 7))) = -37/150
```

1.3
```
(defn square [x] (* x x))

(defn maxofthree [x y z]
  (cond
    (and (< x y) (< x z)) (+ (square y) (square z))
    (and (< y x) (< y z)) (+ (square x) (square z))
    (and (< z y) (< z x)) (+ (square y) (square x))
    )
)
```

1.4

Functions and predefined constructions in LISP can return other functions and operators. So here IF returns operaton (- or +) and then this operator is applied to function arguments

1.5

When we need to evaluate argument the program hangs. So in applicative order we will have an error, in normal order the expression evaluates as 0.
