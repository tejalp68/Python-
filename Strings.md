# Strings

## 1. What strings are in Python

Strings are **immutable sequences of characters**. Any "modification" actually creates a new string object.

```python
s = "Hello"
s[0] = "h"   # TypeError: 'str' object does not support item assignment
```

## 2. Creating strings

```python
s1 = 'single quotes'
s2 = "double quotes"
s3 = '''triple quotes
spans multiple lines'''
s4 = "It's a \"quote\""     # escaping
s5 = r"raw\nstring"          # raw string — \n stays literal, not a newline
```

## 3. Indexing & Slicing (core DSA skill)

```python
s = "Hello World"

s[0]        # 'H'  -----> s.__getitem() this is dunder function which get called when we do sliciing   so s[i] = s.__getitem__(i)
s[-1]       # 'd'  (negative index = from end)
s[0:5]      # 'Hello'
s[:5]       # same as above   default 0 as start index
s[6:]       # 'World'      default end +1  as ending
s[:]        # full copy
s[::2]      # every 2nd char -> 'HloWrd'
s[::-1]     # reversed string -> 'dlroW olleH'
s[-3:]      # last 3 chars -> 'rld'
s[2:8:2]    # start:stop:step -> 'lo o'
```

`s[::-1]` for reversal is asked in almost every string interview problem.

Because the step is negative, Python doesn't use the normal defaults (0 to end). Instead, when step < 0:

start defaults to the last index (len(s) - 1)
stop defaults to one before index 0 (conceptually, "off the left edge")

So `s[::-1]` effectively means: "start at the last element, and walk backward to the first element, inclusive."

## 4. String Methods — the ones you'll actually use

**Case:**
```python
s.lower()        # "hello world"
s.upper()        # "HELLO WORLD"
s.title()        # "Hello World"
s.capitalize()   # "Hello world"
s.swapcase()     # "hELLO wORLD"
```

**Whitespace/cleanup:**
```python
"  hi  ".strip()    # "hi"   — both sides
"  hi  ".lstrip()   # "hi  " — left only
"  hi  ".rstrip()   # "  hi" — right only
```

**Searching:**
```python
s.find("World")     # 6 (index) or -1 if not found — doesn't raise error
s.index("World")    # 6 (index) or raises ValueError if not found
s.count("o")        # 2 — number of occurrences
s.startswith("Hello")
s.endswith("ld")
```

**Splitting & Joining (very common in interviews):**
```python
"a,b,c".split(",")        # ['a', 'b', 'c']
"hello world".split()      # ['hello', 'world']  — splits on any whitespace
"a  b   c".split()          # ['a', 'b', 'c']    — collapses extra spaces
",".join(["a","b","c"])      # "a,b,c"
"".join(["a","b","c"])        # "abc"  — common way to build a string from a list of chars
```

**Replacing:**
```python
s.replace("World", "Python")     # replaces ALL occurrences
s.replace("o", "0", 1)            # replace only first occurrence
```

**Checks (return bool — great for validation logic):**
```python
"123".isdigit()      # True
"abc".isalpha()       # True
"abc123".isalnum()     # True
"   ".isspace()         # True
"Hello".isupper()        # False
"HELLO".isupper()         # True
```

## 5. String Formatting — 3 ways (know all 3, f-strings preferred)

```python
name, age = "Sam", 25

# 1. % formatting (old style, still appears in legacy code)
"%s is %d years old" % (name, age)

# 2. .format() method
"{} is {} years old".format(name, age)
"{0} is {1}".format(name, age)   # positional

# 3. f-strings (modern, fastest, preferred)
f"{name} is {age} years old"
f"{3.14159:.2f}"        # "3.14" — 2 decimal places
f"{1000000:,}"           # "1,000,000" — thousands separator
f"{name!r}"                # repr() version, adds quotes: 'Sam'
```

## 6. String Concatenation & Repetition

```python
"Hello" + " " + "World"   # "Hello World"
"ab" * 3                    # "ababab"

# Efficient concatenation in a loop — DON'T do this:
result = ""
for word in words:
    result += word           # creates a new string object every time — O(n²)

# DO this instead:
result = "".join(words)      # O(n) — much more efficient, ask about this in interviews
```

## 7. Common DSA String Patterns

```python
# Check palindrome
s = "racecar"
s == s[::-1]     # True

# Count character frequency
from collections import Counter
Counter("aabbbc")   # Counter({'b': 3, 'a': 2, 'c': 1})

# Check anagram
sorted("listen") == sorted("silent")   # True

# Remove duplicates while preserving order
list(dict.fromkeys("aabbcc"))    # ['a', 'b', 'c']

# Convert string <-> list of chars
list("hello")          # ['h','e','l','l','o']
"".join(['h','e','l','l','o'])   # 'hello'

# Convert string <-> ASCII
ord('a')     # 97
chr(97)      # 'a'
```

## 8. String Comparison

Strings compare lexicographically (character by character, using ASCII/Unicode values):
```python
"apple" < "banana"    # True  ('a' < 'b')
"Apple" < "apple"     # True  (uppercase letters have lower ASCII values than lowercase)
"10" < "9"             # True  — string comparison, NOT numeric! ('1' < '9')
```

**Gotcha:** comparing numeric strings compares character-by-character, not by value — a classic interview trap.

---

### Quick check
What do these print?
```python
print("Hello"[1:4])
print("abc" * 2)
print(sorted("dcba"))
print("  Hi  ".strip().lower())
```
