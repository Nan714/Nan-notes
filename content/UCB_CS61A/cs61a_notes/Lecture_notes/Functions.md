---
course: cs61A
---

# Names, Assignment, and User-Defined Functions
terminal: python3
control +L : clear 

bind names to values
 1. import (names that built in)
 2. assignment statement
 3. def statement (create new function)

-------
# Environment Diagrams
python tutor :  see the procedure
### Execution rule for assignment statements
1. Evaluate all expressions to the right of = from left to right
2. Bind all names to the lest of = to the resulting values in the current frame.

```python
a=1
b=2
b,a = a+b,b
print(a,b)
```
先把a+b，b的结果算出来，再将结果带入b，a 

----------
# Defining functions


```
def <name>(<formal parameter>): 
	return <return expression>
```
### Execution procedure for def statements:
1. Create a function with signature `<name>(<formal parameters>)`
2. Set the body of that function to be everything indented after the first line 
3. Bind `<name>` to that function in the current frame

### Procedure for calling/applying user-defined functions
1. Add a local frame, forming a new environment
2. Bind the function's formal parameters to its arguments in that frame
3. Execute the body of the function in that new environment![[截屏2026-01-18 23.36.19.png]]
A function's signature has all the information needed to create a local frame
### Looking up names in environments
Every expression is evaluated in the context of an environment
So far, the current environment is either:
- the global frame alone, or
- A local frame, followed by the global frame

### 2 important things
**1. An environment is a sequence of frames
2.A name evaluates to the value bound to that name in the earliest frame of the current environment in which that name is found**（先local frame，再global frame)

#environment 

---------

# Print and None
### None indicates that nothing is returned
- **None** represents **nothing** in Python
- A function that does not explicitly return a value will return **None**
- *Careful*: **None** is not displayed by the interpreter as the value of an expression
```python
def does_not_square(x):
	x*x #No return
	
does_not_square(4)# None value is not displayed
sixteen = does_not_square(4)
sixteen + 4
#TypeError: unsupported operand type(s) for +: 'NoneType' and 'int'
```
### Pure functions & Non-Pure Functions
![[截屏2026-01-19 07.55.10.png]]
### Nested expressions with print
```python
>>>print(print(1),print(2))
1
2
None None
```
![[截屏2026-01-19 07.58.33.png]]
#none

---------

# Miscellaneous Python Features
add()
mul()
from operator import truediv(/), floordiv(//)

def divide_exact(n, d=10):
d不填数字的话默认是10
