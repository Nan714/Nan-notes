# Exceptions

![[截屏2026-07-28 16.56.59.png]]

```python
def double(x): 
	raise TypeError('Double only takes numbers')
	2*x
	
double(2)
double('hello') # TypeError
```

### Try statement

![[截屏2026-07-28 17.08.23.png]]

![[截屏2026-07-28 17.14.27.png]]

---
# Example: Reduce
```python
def reduce(f,s,initial):
	"""Combine elements of s using f starting from initial"""
	>>>reduce(mul, [2,4,8],1)
	64
	>>>reduce(add,[1,2,3,4],0)
	10
	"""
	for x in s:
		initial = f(initial,x)
	return initial
#for循环写法

def reduce(f，s，initial):
	if not s:
		return initial
	return reduce(f,s[1:],f(initial,s[0])) #最后的initial负责不断累加/累运算，最后到底了就输出initial，就是最终的结果
```

---

# Programming language

![[截屏2026-07-31 11.48.44.png]]

![[截屏2026-07-31 11.51.43.png]]

---

# Calculator

scheme_reader.py是用来读取的
scalc.py是用来运算的 （scalc: scheme-syntax calculator


![[截屏2026-07-31 12.02.27.png]]

### Calculator Syntax

![[截屏2026-07-31 12.06.53.png]]

Syntax只包括语法，但实际上+ -等等符号代表的意义（+ -是做什么的 代表什么）并不属于语法范畴，属于semantics

### Calculator semantics

![[截屏2026-07-31 12.09.25.png]]

![[截屏2026-07-31 12.18.36.png]]

**Eval evaluates expressions.**

**Apply applies procedures to arguments.**

也就是说：

- **`eval`****（求值）**：把表达式解释成一个值；如果是函数调用，还要先求出函数和所有参数。
- **`apply`****（应用）**：拿着已经求好的函数和参数，真正执行这个函数。


(+ (* 2    3) 4)
        │
        ▼
      eval
        │
        ├─ eval +
        ├─ eval (* 2 3)
        │        │
        │        ▼
        │      apply *
        │        │
        │        ▼
        │        6
        │
        ├── eval 4
        ▼
函数：+
参数：[6, 4]
        │
        ▼
     apply +
        │
        ▼
        10