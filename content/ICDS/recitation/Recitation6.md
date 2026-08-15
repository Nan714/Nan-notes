### How python execute codes
```python
class Base:
    def __init__(self):
        self.setup()

    def setup(self):
        print("Base setup")


class Child(Base):
    def setup(self):
        print("Child setup")
        
        
c = Child()
```
创建对象时：
c = Child()
python 做的事情是：
1. 创建一个Child类型的对象
2. 调用 Base.init(c)
	1. Child 没有定义__ init__， 所以会用Base.\_\_init__
3. 但type(c) == Child, 所以`self.setup()` 等价于`Child.setup(c)`



decorator
```python
def logger(func):
	def wrapper():
		print("Before running")
		result = func
		print("After running")
		return result
	return wrapper #一定要return！
	
	
@decorator
def my_function():
	print("Running function body...")
	return "Done!"
```

1. Define the function my_function()
2. When Python sees `@decorator`, it is equivalent to:
`my_function = decorator(my_function)`
所以说 如果不写
3. When calling my_function(), you’re actually executing wrapper()
4. Inside wrapper(), it first runs the pre-processing logic → calls the original function → then runs the post-processing logic.




```python
def demo(*args,**kwargs):
	print("args:",args)
	print("kwargs:",kwargs)
	
demo(1,2,3,name="Alice",age=19)
```
**name=直接写就好！不用括号！**
**函数括号里面是先args再kwargs，那么下面运行的时候，就要遵循这个顺序**
eg. 不能demo(1,2,name="Alice",3) or demo(name="Alice",1,2)


### JSON

| **函数** | **输入**   | **输出**   | **用途**    |
| ------ | -------- | -------- | --------- |
| dumps  | Python对象 | 字符串      | 转成JSON字符串 |
| dump   | Python对象 | 文件       | 写入JSON文件  |
| loads  | 字符串      | Python对象 | 解析JSON字符串 |
| load   | 文件       | Python对象 | 读取JSON文件  |
1. 只能双引号
2. 最后一个元素后不能有逗号
3. 不能写注释
4. key必须是字符串（有双引号


webAPI
configuration files配置文件


什么时候需要加a=？（给出一个新变量 给它赋值
看返回值是什么
如果是None 就不用
如果有返回值 大概率需要赋值
但也有例外 eg.  .pop可以不赋值 如果你不需要的话



```python
print(a := 1)
2
```


运行逻辑

堆 栈 堆栈
大部分：从里到外 函数+值 得到新值，再给外层函数
另一种：当成字符串划分



```python

class A:
	def


#a 是A中一个object
a.get(distance)
A.get(a,distance) #其实在干这件事
```