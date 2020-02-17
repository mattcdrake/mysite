---
title: "SICP - Section 1.2"
date: 2019-12-01T20:04:16-05:00
draft: false
---

# Section 1.2 - Procedures and the Processes they Generate

You can see all of my code [here](https://github.com/mattcdrake/sicp).

## Exercise 1.9

```scheme
; Version 1
(define (+ a b)
    (if (= a 0)
        b
        (inc (+ (dec a) b))))

; Version 2
(define (+ a b)
    (if (= a 0)
        b
        (+ (dec a) (inc b))))
```

Version 1 expands in a way that is linearly recursive. The space requirement is
a factor of n where n is the number supplied as the formal parameter `a`.

```scheme
(+ 4 5)
(inc (+ 3 5))
(inc (inc (+ 2 5)))
(inc (inc (inc (+ 1 5))))
(inc (inc (inc (inc (+ 0 5)))))
(inc (inc (inc (inc 5))))
(inc (inc (inc 6)))
(inc (inc 7))
(inc 8)
9
```

Version 2 is a linearly iterative process and the space requirement is constant
regardless of how n scales.

```scheme
(+ 4 5)
(+ 3 6)
(+ 2 7)
(+ 1 8)
(+ 0 9)
```

## Exercise 1.10

I'm not going to expand all 3 but here's the first.

```scheme
(A 1 10)
(A 0 (A 1 9))
(A 0 (A 0 (A 1 8)))
(A 0 (A 0 (A 0 (A 1 7))))
(A 0 (A 0 (A 0 (A 0 (A 1 6)))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 1 5))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 4)))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 3))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 2)))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 1 1))))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 2)))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 4))))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 (A 0 8)))))))
(A 0 (A 0 (A 0 (A 0 (A 0 (A 0 16))))))
(A 0 (A 0 (A 0 (A 0 (A 0 32)))))
(A 0 (A 0 (A 0 (A 0 64))))
(A 0 (A 0 (A 0 128)))
(A 0 (A 0 256))
(A 0 512)
1024

(define (f n) (A 0 n)
; 2n

(define (g n) (A 1 n)
; 2^n

(define (h n) (A 2 n)
; Unsure how to represent this using _concise mathematical definitions_.
; 2^2^2... etc (number of 2's = n)
```

