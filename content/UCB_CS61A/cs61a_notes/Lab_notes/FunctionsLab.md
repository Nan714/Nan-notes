### Debugging Techniques
detailed info: <https://insideempire.github.io/CS61A-Website-Archive/articles/debugging/index.html>

#### running doctests

```
def foo(x):
    """A random function.

    >>> foo(4)
    4
    >>> foo(5)
    5
    """
```

The lines in the docstring that look like interpreter outputs are the **doctests**. To run them, go to your terminal and type:

```
python3 -m doctest file.py
```

The command line tool has a `-v` option that stands for _verbose_.
```
python3 -m doctest file.py -v
```
In addition to telling you which doctests you failed, it will also tell you which doctests passed.

