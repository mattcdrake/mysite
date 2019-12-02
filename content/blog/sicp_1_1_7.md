---
title: "SICP - Section 1.1.7"
date: 2019-12-01T19:54:12-05:00
draft: false
---

# SICP Notes & Exercises - Starting at 1.1.7

I started reading SICP this week. I found Eli Bendersky's series of SICP 
[reading notes](https://eli.thegreenplace.net/tag/sicp) while looking for 
exercise answers to compare mine to and thought it was a fantastic idea. Also 
this week, Nadia Eghbal's blog post about making her own PhD introduced me to 
the idea of "learning in public". So I'm going to synthesize these two ideas 
and document some of my own thoughts and exercise solutions.

Disclaimer: aside from a brief agent-based modeling stint involving NetLogo,
this is my first real exposure to any kind of Lisp. __I am not a Lisp expert__ 
(or intermediate) and you should not treat these solutions as a reference.

You can see all of my code [here](https://github.com/mattcdrake/sicp).

## Exercise 1.6

The program will result in infinite recursion thanks to applicative-order 
evaluation. `else-clause` is evaluated prior to applying the `new-if` 
procedure. As a result, `else-clause` gets evaluated before the conditional is
tested. This pattern happens recursively without any chance to break the cycle. 

## Exercise 1.7

This method performs poorly with very small numbers due to the comparison value
used in `good-enough?`. Given a guess <= x, the procedure will return on the
first try for any x smaller than the comparison value (0.001 in the example).

This straightforward example will fail due to numbers being smaller than the
threshold.

```scheme
(sqrt-iter 0.001 0.001)
```

My initial explanation for large numbers failing this test had to do with the 
size of integers. I assumed that similar to C++, an integer would have a max
limit that it was capable of holding. According to the internet, that isn't the
case - it seems that isn't the case and Scheme is capable of dealing with any
integer size provided you have memory for it.

That being said, I think that my hypothesis has something to do with it. Given
the following examples, the first hangs and the second has a "not in the
correct range" error.

Update: After working through the rest of the problem, I realize now that the
shortcoming related to large numbers comes from the impracticality of getting
a diff lower than the comparison threshold when dealing with floating point
math. I'll leave in my wrong train of thought because this is my learning diary
and I can do whatever I want.

```scheme
(sqrt-iter 134452349527039 13258287509283745)

(sqrt-iter 134452349527039847502983475092836478579623984765293874650781603427561083745601983745029837450289347509782364958726394875623948756293487562093784650709857209837450928345260394856273465027345602376450293746502938764502398745620398745239459827340956203495786203984752039847520348965203948562037846502313.0 
           13258287509283745092834509623049567820398475029384658726304985760198346507983640597263048957623984756203465203495862039485720398457203498572304650293746509273645029834570298346502937845209347852798374509283645097263405987619834560718934650197346513948765137945613048756103748560796063987562398745692387465298374652983746502364592837459238745629387562938457934610346510387450138945163045813764501345786203940568273049852673094852376405986234058726034589273654023673475209387450923874523451.0)
```

This is my implementation that returns `#t` when the delta between `old-guess`
and `guess` gets very small relative to `guess`.

```scheme
(define (new-sqrt-iter guess x old-guess)
  (if (new-good-enough? guess x (- old-guess guess))
          guess
          (new-sqrt-iter (improve guess x)
                     x
                     guess)))


; This assumes that the first guess is not exactly right, which probably isn't
; a very good assumption. I fix this in ex. 1.8
(define (new-good-enough? guess x delta)
  (if (and (< (abs (/ delta guess)) 0.01) (not (= delta 0)))
    #t
    (< (abs (- (square guess) x)) 0.001)))
```

It doesn't solve the problem for very small numbers because that issue is 
related to the comparison threshold and not being unable to get to. It does 
help with very large numbers because it doesn't require the kind of precision
that floating point math caused trouble with in the first place.

## Exercise 1.8

Similar solution to 1.7 but I improved some of the code.

```scheme
; Wrapper so you don't have to pass in guess to find "delta" on the first go.
(define (cube-root-iter guess x)
  (cube-root-iter-inner guess x (* guess 2)))

(define (cube-root-iter-inner guess x old-guess)
  (if (good-enough? guess x (- old-guess guess))
    guess
    (cube-root-iter-inner (improve guess x)
                          x
                          guess)))

(define (good-enough? guess x delta)
  (< (abs (/ delta guess)) 0.01))

(define (improve guess x)
  (/ 
    (+ 
      (/ x (expt guess 2)) 
      (* 2 guess))
    3))
```


