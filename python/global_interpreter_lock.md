# Global Interpreter Lock (GIL)

## Process

- processes are execution environments which `don't share state` with each other.

## Threads

- Threads are like processes but they `share state with each other` like `global variables` and so on.

## Reference Count

- The `number` of people that are interested in an object. for example:

```python
a = []
print(sys.getrefcount(a))
>>> 2
b = a
print(sys.getrefcount(a))
>>> 3
```

- On line 1 in the above code we create a python list called `a`, it has reference count of 1 but `sys.getrefcount(a)` prints `2`, because it creates its own reference count during execution and then drops it at the end. Then on line 4 we create another reference to `a` called `b`, now the reference count is `2`.
