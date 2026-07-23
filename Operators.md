**Point 2: Operators**
## 1. Arithmetic Operators

```python
a, b = 7, 2

a + b   # 9   addition
a - b   # 5   subtraction
a * b   # 14  multiplication
a / b   # 3.5 true division (always returns float)
a // b  # 3   floor division
a % b   # 1   modulus (remainder)
a ** b  # 49  exponentiation
```

**Floor division and modulo gotchas with negatives (very commonly tested):**
```python
-7 // 2   # -4  (rounds toward negative infinity, not zero)
-7 % 2    # 1   (result always has the SAME SIGN as the divisor)
7 // -2   # -4
7 % -2    # -1
```
Rule to remember: `a == (a // b) * b + (a % b)` always holds true.

## 2. Comparison (Relational) Operators

```python
a == b   # equal to
a != b   # not equal to
a > b    # greater than
a < b    # less than
a >= b   # greater than or equal
a <= b   # less than or equal
```

These work on strings and other sequences too (lexicographic/alphabetical comparison):
```python
"apple" < "banana"   # True
[1, 2, 3] < [1, 2, 4]  # True — compares element by element
```

**Chained comparisons** (Python-specific, doesn't exist in most languages):
```python
x = 5
print(1 < x < 10)     # True — same as (1 < x) and (x < 10)
print(10 < x < 1)     # False
```

## 3. Logical Operators

```python
a and b   # True only if both are True
a or b    # True if at least one is True
not a     # inverts the boolean
```

Remember from earlier: `and`/`or` return actual values, not just `True`/`False` (short-circuit evaluation):
```python
5 and 0    # 0
5 and 3    # 3
0 or 5     # 5
```

## 4. Assignment Operators

```python
x = 5
x += 1   # x = x + 1
x -= 1   # x = x - 1
x *= 2   # x = x * 2
x /= 2   # x = x / 2
x //= 2  # x = x // 2
x **= 2  # x = x ** 2
x %= 2   # x = x % 2
```

**Walrus operator `:=`** (Python 3.8+, shows up in interviews to test modern knowledge) — assigns and returns a value in one expression:
```python
# Without walrus
n = len(data)
if n > 10:
    print(n)

# With walrus
if (n := len(data)) > 10:
    print(n)

# Common use in loops
while (line := input()) != "quit":
    print(line)
```

## 5. Bitwise Operators (commonly tested for DSA/interviews)

```python
a, b = 5, 3   # binary: 101, 011

a & b    # 1    AND   -> 001
a | b    # 7    OR    -> 111
a ^ b    # 6    XOR   -> 110
~a       # -6   NOT   -> inverts bits (equivalent to -(a+1))
a << 1   # 10   left shift  (multiplies by 2^n)
a >> 1   # 2    right shift (integer divides by 2^n)
```

**Interview tricks using bitwise ops:**
```python
# Check if a number is even/odd (faster than %)
n & 1 == 0   # True if even

# Swap two numbers without a temp variable
a, b = 5, 3
a = a ^ b
b = a ^ b
a = a ^ b

# Check if power of 2
n & (n - 1) == 0   # True if n is a power of 2 (and n != 0)

# XOR trick — find the single non-duplicate number in an array where everything else appears twice
result = 0
for num in [4, 1, 2, 1, 2]:
    result ^= num
# result == 4
```

## 6. Identity Operators

```python
a is b       # True if same object in memory
a is not b   # True if different objects
```

Covered earlier — remember `is` checks identity, `==` checks value equality. Use `is` mainly for `None` checks:
```python
if x is None:      # correct, Pythonic
    ...
if x == None:       # works but not idiomatic
    ...
```

## 7. Membership Operators

```python
"a" in "abc"          # True
3 in [1, 2, 3]         # True
"key" in {"key": 1}     # True — checks keys for dict
"key" not in {"a": 1}    # True
```

These are O(n) for lists but O(1) average for sets/dicts — **important for DSA time complexity questions.**

## 8. Operator Precedence (order of evaluation)

From highest to lowest (the ones that matter most for interviews):

```
**                  (exponent)
~ + -               (unary)
* / // %            (multiplicative)
+ -                 (additive)
<< >>               (bitwise shift)
&                   (bitwise AND)
^                   (bitwise XOR)
|                   (bitwise OR)
== != > < >= <=      (comparisons)
not
and
or
```

```python
print(2 + 3 * 4)        # 14, not 20
print(2 ** 3 ** 2)       # 512, ** is right-associative: 2**(3**2), not (2**3)**2
print(not True and False)  # False — not binds tighter than and
```

---

### Quick check
What do these print?
```python
print(-7 % 3)
print(5 & 6)
print(2 ** 2 ** 3)
print(3 or 4 and 0)
```
