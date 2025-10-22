### **Uses of Regex** 
- Used to **Validate** Text 
- Used to **Search** through text 

--- 

### **Setup**

```python
import re #Import regex
```

--- 

### **Replacing characters using Regex 
`re.sub(pattern, replacement, string)`
- **pattern** → the regex rule to match.
    
- **replacement** → what to replace matches with.
    
- **string** → the original text.
#### Example: 

```python
import re
text = "abc123"
result = re.sub(r'\d', '', text)  # remove digits
print(result)  # "abc"
```


### **Validating an Email 
```python
import re
 
```


