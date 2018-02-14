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