Table of Content
================
- [Code Python get OS type](#code-python-get-os-type)
- [Code Python write file](#code-python-write-file)

## Code Python get OS type 
```python  
from sys import platform  
if platform == "linux" or platform == "linux2":  
# Todo in linux  
elif platform == "darwin":  
# Todo in OSx  
elif platform == "win32":  
# Todo in Windows 
```

## Code Python write file
```python
file = open('filename.txt','w')
file.write("content")
file.close()
```
