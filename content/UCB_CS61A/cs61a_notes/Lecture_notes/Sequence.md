# For statement

```python
l = [[1,2],[1,1],[2,3],[2,2]]
same = 0
for x,y in l: #可以直接用x，y指代list里的东西
	if x == y:
		same +=1
		
print(same)
```

如果你不想要i 只想要重复多少次 可用 **\_** 代替
```python
for _ in range(3):
	print("GO")
`