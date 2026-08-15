# Numpy
- NumPy stands for “**Numerical Python**”
    
- It provides an efficient multidimensional array object (ndarray)
    
- It is mainly responsible for matrix computation, distance calculation, and feature manipulation in machine learning

NumPy can operate on entire arrays at once, which makes it much faster than using for loops.

```python
import numpy as np
a = np.array([[1,2,3],[4,5,6]])

print(a)
print(a.shape) #(2,3)
print(a.ndim) #2
print(a.dtype) #int64
```

```python
print(np.zeros((2,3))) #全0矩阵

#[[0. 0. 0.]

# [0. 0. 0.]]

print(np.ones((2,3))) #全1矩阵

#[[1. 1. 1.]

# [1. 1. 1.]]

print(np.eye(3)) #单位矩阵

#[[1. 0. 0.]

# [0. 1. 0.]

# [0. 0. 1.]]

print(np.random.rand(2,3)) #随机矩阵

#[[0.5488135 0.71518937 0.60276338]

# [0.54488318 0.4236548 0.64589411]]

print(np.random.randint(0,10,(2,3))) #随机整数矩阵

#[[3 7 9]

# [3 5 2]]

print(np.random.choice([1,2,3],(2,3))) #随机选择矩阵

#[[3 1 3]

# [1 1 2]]

print(np.random.choice([1,2,3],(2,3),p=[0.2,0.5,0.3])) #随机选择矩阵，带概率

print(np.arange(0,10,2)) #等差数列

#[0 2 4 6 8]

print(np.linspace(0,1,5)) #等差数列，指定元素个数

#[0. 0.25 0.5 0.75 1. ]
```
np.arange(start, stop, step) creates values from start (inclusive) to stop (exclusive), increasing by step.

np.linspace(start, stop, num) creates num evenly spaced points between start and stop (both inclusive).

### Indexing and slicing

```python
m = np.arange(12).reshape(3,4)

print(m)

#[[ 0 1 2 3]

# [ 4 5 6 7]

# [ 8 9 10 11]]

print(m[0,2]) #2

print(m[1,:]) #[4 5 6 7]

print(m[1:,:2])

#[[4 5]

# [8 9]]

print(m[:2,1:3])

#[[1 2]

# [5 6]]

print(m[m>5]) #[ 6 7 8 9 10 11]

print(m[m%2==0]) #[ 0 2 4 6 8 10]
```

### Broadcasting
Broadcasting allows arrays with different shapes to participate in computations.
For broadcasting, NumPy aligns from the right
```python
a = np.array([1,2,3])

b = np.array([4,5,6])

  

print(a+b) #[5 7 9]

print(a*2) #[2 4 6]

print(a*b) #[ 4 10 18]

print(a+5) #[6 7 8]
```

### mathematical functions
These are used for data analysis and normalization operations.
```python

a = np.array([1,2,3])

b = np.array([4,5,6])

print(np.sum(a)) #6

print(np.mean(a)) #2.0

print(np.mean(a, axis=0)) #2.0

#axis=0表示对列进行操作(每一列的平均值），axis=1表示对行进行操作
#这里是对一维数组a进行操作，所以axis=0和axis=1的结果是一样的 为什么？因为a是一维数组，只有一个轴，所以无论是axis=0还是axis=1，都会对整个数组进行操作，得到相同的结果。

print(np.std(a)) #0.816496580927726

print(np.linalg.norm(a)) #3.7416573867739413

#np.linalg.norm(x) = sqrt(sum(x_i^2)) 默认是L2范数 linalg = linear algebra

print(np.linalg.norm(a, ord=1)) #6

#ord=1表示L1范数 sum(abs(x_i)) ord是norm的缩写

print(np.var(a)) #0.6666666666666666

print(np.median(a)) #2.0

print(np.max(a)) #3

print(np.min(a)) #1

print(np.sqrt(a)) #[1. 1.41421356 1.73205081]

print(np.exp(a)) #[ 2.71828183 7.3890561 20.08553692]

#exp(x) = e^x

print(np.log(a)) #[0. 0.69314718 1.09861229]

#log(x) = ln(x)

print(np.clip(a+5,0,8))

#clip(x, a_min, a_max) 将数组中的元素限制在a_min和a_max之间，超过a_max的元素被设置为a_max，低于a_min的元素被设置为a_min。这里是将scores中的每个元素加5后，限制在0和100之间。
```

### matrix operations

NumPy supports matrix operations, like dot product, matrix multiplication, transpose, and inverse matrix operations

Commonly used in linear algebra computations for machine learning
```python
A = np.array([[1,2],[3,4]])

B = np.array([[2,0],[1,2]])

  

print(A.dot(B))

print("Matrix multiplication:\n", np.dot(A,B))

print(A@B) #@是Python 3.5引入的矩阵乘法运算符，等价于np.dot(A,B)

#[[4 4]

# [10 8]]

print("Transpose:\n",A.T)

#[[1 3]

# [2 4]]

print("Inverse:\n",np.linalg.inv(A))

#[[-2. 1. ]

# [ 1.5 -0.5]]

print("A multiplied by its inverse:\n",A.dot(np.linalg.inv(A)))

#[[1. 0.]

# [0. 1.]]

print(np.mean(A,axis = 0))
print(np.mean(A,axis = 1))
print(np.linalg.norm(A))
#norm是指矩阵的范数，表示矩阵的大小或长度。默认是L2范数，即sqrt(sum(A_ij^2))，也就是矩阵中所有元素的平方和的平方根。
```

```python
import numpy as np
A = np.array([[1,2,3],[4,5,6]])
print(A)

A.reshape(-1)
print(A)
A.reshape()
```

**`reshape(-1)`**

in NumPy / PyTorch

`reshape(-1)` means:

Flatten the array into 1 dimension automatically.