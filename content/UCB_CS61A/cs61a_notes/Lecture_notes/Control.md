# Multiple Environments
### Life cycle of a user-defined function
![[截屏2026-01-23 17.21.37.png]]

### Names have no meaning without environments
![[截屏2026-01-23 17.34.08.png]]
**An environment is a sequence of frames.**
先找earliest的 再慢慢往后推

### Names have different meanings in different environments
![[截屏2026-01-23 17.39.28.png]]
**A call expression and the body of the function being called are evaluated in different environments**

-------

# Conditional statements
### Statements
A *statement* is executed by the interpreter to perform an action 
![[截屏2026-01-23 17.44.11.png]]

A *suite* is a sequence of statements

To *execute* a suite means to execute its sequence of statements, in order

Execute rule for a sequence of statements:
- Execute the first statement
- Unless directed otherwise, execute the rest

### Conditional statements
![[截屏2026-01-23 17.50.15.png]]

### Boolean Contexts
False: False, 0, '', None (more to come) （第三个是空的string）
True: Anything else (True)

-----

# Iteration
### While statements
![[截屏2026-01-23 17.56.44.png]]