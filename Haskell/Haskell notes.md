### Haskell notes

Load module: 

```haskell
module MoudleName where 
say = "Hello"
:load ModuleName
```

Call function:

```haskell
foo bar --used for calling function foo with argument bar
acos (cos pi) --call acos with result of calling function cos with argument pi
max 5 42 --calling multiple arguments
(max 5) 42 -- equvivalent expression, max 5 return something like partial function of 1 argument, which is called on 42

-- function of N elementes used with 1 element returns function of N-1 elements (currying)

```

Conditional expression:

```haskell
f x = (if x > 0 then 1 else (-1)) + 5
-- both if and else are necessary
```

Operators:

```haskell
-- call function like an operator
5 `max` 6 -- 6
-- use operator like a function (all operators in haskell are binary excluding minus)
(+) 6 7
```

Priority:

All operators have priority in range 0..9. Any function has priority 10

```haskell
-- Left associative operators (value is calculated from left to right):
infixl 6 +, - 
-- Right associative
infixr 8 ^, `logBase` -- 2 ^ 3 ^ 2 = 512, but not 64 (from right to left)
```

Custom operator

```haskell
infixl 6 *+*
a *+* b = a ^ 2 + b ^ 2 
-- or
(*+*) a b = a ^ 2 + b ^ 2
-- there is no difference in usage, after we define the operator (we can use it both in functional and operator way)
```

Operators conjunction

```haskell
(/) 2 == (2 /)
(2 / ) 4 = 0.5
(/ 2) 4 = 2
-- NOTE: it works only for operators, functions can not be curried from right to left
```

Remove unnecessary brackets:

```haskell
f $ x = f x
sin 0 == sin $ 0 
sin (pi / 2) == sin $ pi / 2
```

