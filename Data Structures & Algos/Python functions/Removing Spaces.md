### 1. **Remove All Spaces**

Removes every space character from the string.

```python 
text = "Hello World"

no_spaces = text.replace(" ", "")
```

---

### 2. **Remove Leading and Trailing Spaces**

Removes whitespace from the beginning and end of the string.

```python 
text = "  Hello World  "

cleaned = text.strip()
```

--- 

### 3. **Remove Leading Spaces Only
**
```python 
text = "   Hello"

cleaned = text.lstrip()
```

---

### 4. Remove Trailing Spaces Only 

```python
text = "Hello   "

cleaned = text.rstrip()
```

---

### 5. **Remove All Whitespace (including tabs, newlines)**

Using regex to remove all whitespace characters.

```python 
import re

text = "Hello \tWorld\n"

cleaned = re.sub(r"\s+", "", text)
```

---

### 6. **Remove Multiple Spaces Between Words**

Replace multiple spaces with a single space.

```python 
import re

text = "Hello     World"

normalized = re.sub(r"\s{2,}", " ", text)
```

---

### 7. **Remove Spaces from a List of Strings** 

```python 
words = [" Hello ", " World "]

cleaned = [word.strip() for word in words]
```

---

### 8. **Remove Spaces in a File or Line-by-Line**

```python 
with open("file.txt") as f:

    lines = [line.replace(" ", "") for line in f]
```