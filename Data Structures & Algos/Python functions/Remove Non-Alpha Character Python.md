### 1.  **Keep only letters (remove everything else)**

```python 
import re

text = "Hello, World! 123_+"
cleaned = re.sub(r'[^A-Za-z]', '', text)
print(cleaned)  # HelloWorld

```

### Breakdown:

- `[...]` → a **character class** (set of characters to match).
    
- `A-Z` → uppercase letters.
    
- `a-z` → lowercase letters.
    
- `^` at the start **inside brackets** → means "NOT these characters."
    
- `[^A-Za-z]` → means “any character that is **not a letter**.”
    
- `''` (empty string) → means we’re deleting those characters.

--- 

### 2. **Keep spaces too**

```python
cleaned = re.sub(r'[^A-Za-z ]', '', text)
print(cleaned)  # Hello World

```

---

### 3. **Using str.isalpha()** 

```python 
text = "Hello, World! 123_+"
cleaned = ''.join(c for c in text if c.isalpha())
print(cleaned)  # HelloWorld

```