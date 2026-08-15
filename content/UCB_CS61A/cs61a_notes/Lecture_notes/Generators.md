**Generator（生成器）是一种特殊的 Iterator（迭代器）。**  
**所有 generator 都是 iterator，但不是所有 iterator 都是 generator。**

Iterator（迭代器）
│
├── list_iterator
├── tuple_iterator
├── dict_keyiterator
├── ...
└── Generator（生成器）

**Iterator 是一种对象，它可以一个一个地产生数据。**
**list收集最外层generator产生的值，也就是说过程中yield只返回上级，不会出现在list中** 

它必须实现两个方法：

	- `__iter__()`
	- `__next__()`

a = next（b） 这个a是一个行动 往后移一个的行动

Generator 是 **Python 自动帮你写好的 iterator。**

![[截屏2026-07-06 16.23.16.png]]

### yield from
```python
def countdown(k):
	if k > 0:
		yield k
		# yield countdown(k-1)这样是不行的！因为这就返回了一个generator而不是数值
		
		# for x in count down(k-1):
		# 	yield x
		# 以上可以写为
		yield from countdown(k-1)	
	else:
		yield 'Blast off'
		
		
			
			
```

![[截屏2026-07-06 18.31.06.png]]

---
# Example: partitions


![[截屏2026-07-06 18.38.53.png]]

```python
'''hw 05'''
if label(t) == value:

	yield [value]

for b in branches(t):

	for path in yield_paths(b, value):

		yield [label(t)] + path

  

# 不能直接yield [label(t)] + yield_paths(b, value)!因为后面这个是generator，不能用list➕generator！

# 我们要相加的是generator得出来的结果，所以要for path in yield_paths
```