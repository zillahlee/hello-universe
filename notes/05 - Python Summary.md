---
favorited: true
tags: [Mine]
title: 05 - Python Summary
created: '2020-03-12T03:13:46.578Z'
modified: '2020-03-14T14:33:44.592Z'
---

# 05 - Python Summary

- `import module as alias` / `from module import object`
- <kbd>Ctrl+C</kbd> interrupts the running code
- four-space indentations, instead of <kbd>Tab</kbd>
- *escape character* `\`

## Scalar Types
`None`, `str`, `bytes`
`int`, `float`, `bool`

`datetime` module
`datetime`, `date`, `time`, `timedelta`

**type casting**
`str()`, `int()`, `float()`, `bool()`
- if `not isinstance(x, list)` AND `isiterable(x)`:
  - `x = list(x)` will convert `x` to a `list`

#### Object & Type
- everything in python is an object
- variables are names for objects within a particular namespace
- the type info is stored in the object itself
- a variable's referring object can change, thus the variable's type can change (but the object itself does not change type)
- `is instance(object, type)` -> `TRUE/FALSE`

## Binary Operators

`a + b`, `a - b`
`a * b`, `a / b`
`a ** b`, `a // b`, `a % b`

`a & b`, `a AND b`
`a | b`, `a OR b`
`a ^ b`, `a EXCLUSIVE-OR b`

`a == b`, `a != b`
`a < b`, `a > b`
`a <=b`, `a >=b`
`a is b`, `a is not b`
*chain comparisons*
- `is` compares whether two variables refer to the same **one object**
- `==` compares whether the **two objects** are equal

## Shortcuts

#### Tab Completion
- press <kbd>Tab</kbd> to search the namespaces of any variable, including methods, attributes, and modules after `object.`
- <kbd>Tab</kbd> Completion also helps with file paths, and the completion of function keyword arguments

#### Help with `?`
- `?` before/after a variable displays general info about it
- if that variable contains/refers to a function/instance method, the docstrings will also show up; `??` adds source code to the description information
- `?` combined with wildcard `*` shows all matching names
  - e.g., `np.*load*?` -> a list of matching methods/modules

#### "Stop" a Function
- `continue` skips the remainder and advances to the next iteration
- `break` exits the innermost loop
- `pass` is used where no action is to be taken, or as a placeholder

#### Ternary Expression (equivalent)
- `val = 'Non-neg' if x >=0 else 'Neg'`
```python
if x >= 0:
  val = 'Non-neg'
else:
  val = 'Neg'
```

#### Miscellaneous
- `range(start, end, step)`
  - `range(0, 20, 2)`
  - `range(5, 0, -1)`
  - `range(len(sequence))`



