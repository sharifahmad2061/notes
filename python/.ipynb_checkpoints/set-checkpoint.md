# Python sets

- A python set is backed by a dictionary which is a hash table internally.
- There `lookup in set is in constant O(1) time`.
- A python set can be created using the following 2 ways:

## Method 1

```python
s = set([1,2,3,4,5])
```

## Method 2

```python
s = {1,2,3,4,5,6}
```

- An empty set **cannot** be created with the `empty curly braces {}`, for an empty set we need to use the `set constructor (i.e. set())`.

- Below are some neat tricks with python sets:

```python
vowels = {'a', 'e', 'i', 'o', 'u'}
letters = set('alice')
letters
```
