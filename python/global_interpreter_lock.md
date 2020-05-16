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

On line 1 in the above code we create a python list called `a`, it has reference count of 1 but `sys.getrefcount(a)` prints `2`, because it creates its own reference count during execution and then drops it at the end. Then on line 4 we create another reference to `a` called `b`, now the reference count is `2`.

## Reference Count & Threads

When working with threads, who work on the same object, it often leads to `race conditions` and the reference count either gets **0** before it shouldn't or stays **> 0** after it shouldn't. This results in people losing reference to objects, they should have access to, or object staying in memory, when it should have been deleted.

## Lock

- In order to solve the problem of race conditions we use the phenomenon of locks.
- The concept of lock is this:
  - Only 1 person can hold it at a time.
  - The person holding the lock can access its associated object.

## Deadlock

- deadlock happens when we have multiple locks and threads acquire them in the opposite order. Now these threads can never move forward, they have gotten stuck in that condition.

## GIL

- It is a `single lock variable` that `must be held by an object before doing anything with the python interpreter`, like `allocating memory`, `running bytecode` etc.

## Ramifications of GIL

### For IO Bound Programs

- If our programs, spends most of its time, waiting for reading from a file, writing to a socket, listening for a socket connection, then `it will work fine under GIL`.

### For CPU Bound Programs

- If all of our threads want to compute something, they all have to wait for the GIL, which essentially `makes our program single threaded`.
