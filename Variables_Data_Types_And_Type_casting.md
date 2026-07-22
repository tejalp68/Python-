# Variables, Data Types & Type Casting

## 1. Variables

Python is dynamically typed — no need to declare a type, it's inferred.

```python
x = 10           # int
name = "Sam"     # str
pi = 3.14        # float
is_valid = True  # bool
```

```python
x = 5
y = x  # y gets a copy of the reference, not a new object (matters for mutable types)

# Multiple assignment
a, b, c = 1, 2, 3
a = b = c = 0        # all point to same value
a, b = b, a           # swap without temp variable — very common in interviews

# Unpacking with *
first, *rest = [1, 2, 3, 4]     # first=1, rest=[2,3,4]
*rest, last = [1, 2, 3, 4]      # rest=[1,2,3], last=4
```

**Naming rules:** letters, digits, underscore; can't start with digit; case-sensitive; can't use reserved words (`if`, `class`, `for`, etc).

**Constants:** Python has no true constants — convention is `MAX_VALUE = 100` (all caps) to signal "don't change this."

---

## 2. Data Types — Full Picture

| Category | Types | Mutable? |
|---|---|---|
| Numeric | `int`, `float`, `complex` | No |
| Text | `str` | No |
| Sequence | `list`, `tuple`, `range` | list: Yes, tuple/range: No |
| Mapping | `dict` | Yes |
| Set | `set`, `frozenset` | set: Yes, frozenset: No |
| Boolean | `bool` | No |
| Binary | `bytes`, `bytearray` | bytes: No, bytearray: Yes |
| None | `NoneType` | — |

### Mutable vs Immutable (heavily interview-tested)

```python
# Immutable — operations create a NEW object
s = "hello"
s += " world"   # new string object created, old one discarded

# Mutable — operations modify the SAME object
lst = [1, 2, 3]
lst.append(4)   # same object, modified in place
```

Why it matters — passing mutable objects into functions lets the function change the caller's data ("pass by object reference"):

```python
def modify(l):
    l.append(100)

my_list = [1, 2]
modify(my_list)
print(my_list)  # [1, 2, 100] — changed!

def modify_num(n):
    n += 1

x = 5
modify_num(x)
print(x)  # 5 — unchanged, int is immutable
```

---

## 3. Numeric Types in Detail

```python
# int — arbitrary precision, no overflow like C/Java
big = 999999999999999999999999999999 + 1  # works fine

# Division operators — commonly confused
7 / 2    # 3.5   (true division, always float)
7 // 2   # 3     (floor division)
-7 // 2  # -4    (floors toward negative infinity, NOT truncates — common trap!)
7 % 2    # 1     (modulo)
-7 % 2   # 1     (Python's modulo always matches sign of divisor)

# Power
2 ** 10  # 1024

# float precision gotcha (asked often)
0.1 + 0.2  # 0.30000000000000004 — floating point imprecision
round(0.1 + 0.2, 2)  # 0.3
```

---

## 4. Strings — Most-Used Operations

```python
s = "Hello World"

s.lower(), s.upper()
s.strip()               # remove whitespace both ends
s.split()                # -> ['Hello', 'World']
s.split(",")              # split by delimiter
"-".join(["a","b","c"])   # -> "a-b-c"
s.replace("World", "Python")
s.find("World")            # index or -1 if not found
s.index("World")           # index or raises ValueError
s.count("o")               # 2
s.startswith("Hello"), s.endswith("ld")
s[::-1]                    # reverse a string — very common interview trick
s.isdigit(), s.isalpha(), s.isalnum()

# Slicing — critical for DSA
s[0:5]     # "Hello"
s[:5]      # same
s[6:]      # "World"
s[::2]     # every 2nd char
s[-1]      # last char
s[-3:]     # last 3 chars

# f-strings (modern formatting, always use this)
name, age = "Sam", 25
print(f"{name} is {age} years old")
print(f"{3.14159:.2f}")   # "3.14" — formatting decimals
```

Strings are **immutable** — `s[0] = 'h'` throws an error. You must build a new string.

---

## 5. Type Casting — Full Table

```python
int("42")          # 42
int("42", 2)        # base 2 -> 34 (base conversion, interview favorite)
int(42.9)             # 42 (truncates toward zero)
float(42)               # 42.0
str(42)                   # "42"
list("abc")                 # ['a', 'b', 'c']
tuple([1,2,3])                 # (1, 2, 3)
set([1,1,2,3])                    # {1, 2, 3} — dedupes
dict([("a",1),("b",2)])              # {'a': 1, 'b': 2}
```

**Common errors to know:**

```python
int("abc")            # ValueError
int("3.5")             # ValueError — can't directly convert decimal string to int
int(float("3.5"))         # 3 — must go through float first
```

---

## 6. Truthy / Falsy — Full List

Falsy values: `False`, `None`, `0`, `0.0`, `0j`, `""`, `[]`, `()`, `{}`, `set()`, `range(0)`

Everything else is truthy.

```python
if my_list:        # instead of len(my_list) > 0
    ...
```

---

## 7. `is` vs `==` (very commonly asked)

```python
a = [1, 2, 3]
b = [1, 2, 3]
a == b   # True — same values
a is b   # False — different objects in memory

c = a
c is a   # True — same object

# Small int caching gotcha
x = 256
y = 256
x is y   # True (Python caches small ints -5 to 256)

x = 257
y = 257
x is y   # False (usually — outside cache range)
```

