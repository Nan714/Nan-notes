1. 最底层return了不够！要每一层都return！
eg.
```python
def berry_finder(t):
	if label(t) == 'berry':
	
		return True
	
	for b in branches(t):
	
		if berry_finder(b):
	
			return True #一定要写return！ if这一块不能直接替代成berry_finder(b)
	
	return False
```
