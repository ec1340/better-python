## Effictive Python Examples

from "Effective Python" by Brett Slatkin

### Pythonic Thinking 

1. Know which version of python you're using
2. Follow the PEP 8 Style Guide
3. Know the differences between bytes and str
4. Prefer interpolated F-strings over C-style format strings and str.format
5. Write helper functions instead of complex expressions
6. Prefer multiple assignment unpacking over indexing

``` python
# ok
foo[:3], bar = some_function()

# better
*foo, bar = some_function()

```
7. Prefer enumarate over range

``` python 
 # ok
for i in range(10):
    ...

# better
for indx, item in enumerate(list_of_things):
    ...
```

8. Use `zip` to process iterators in parallel


9. Prevent repitition with assignment expressions


### Lists and Dictionaries

11. Know how to slice sequences

``` python 
 # implicitly creates a max len to return
 a = ['a', 'b', 'c', ['d']]
 a[:20] # returns all entries
 a[-20:] # returns all entries

 # replacements don't need to be equal in size
 a[:3] = ['e', 'f'] # a = ['e', 'f', 'd']

b = a # copies contents
b = a[:] # allocates a new list
```

12. Avoid striding and slicing in a single expression
``` python

a = ['a', 'b', 'c', 'd']

# get odd entries
a[::2] # start:end:stride

# reverse a list or byte string
x = b'apples'
y= [::-1]

# avoid using all three args
a[1:3:2]
```

13. Prefer catch-all unpacking over slicing
``` python
# ok
oldest = ages_descending[0]
second_oldest = ages_descending[1]
others = ages_descending[2:]

# better
oldest, second_oldest, *others = ages_descending   
```
14. Sort by complex criteria using the `key` parameter
``` python
# list type provides a sort method
numbers = [1,2,5,3,4,0]
numbers.sort()

# sort accepts a key parameter
tools = [toolClass, toolClass, toolClass]
tools.sort(key=lambda x : x.name) 
tools.sort(key=lambda x : (x.name, x.weight)) # can sort by multiple attributes
tools.sort(key=lambda x : (x.name, x.weight), reverse=True)
tools.sort(key=lambda x : (x.name, -x.weight), reverse=True) # can mix orders like that
```

15. Be cautious when relying on dict insertion ordering

16. Prefer `get` over `in`  and `KeyError` to handle missing dictionary keys

17. Prefer `defaultdict` over `setdefault` to handle missing items in internal state 

18. Know how to construct key-dependent default values with `__missing__`

### Functions
19. Never unpack more than three variables when functions return multiple values
20. Prefer raisig exceptions over returning `None`
21. Know how closure interact with variable scope
22. Reduce visual noise with variable positional arguments
23. Provide optional behavior with keyword arguments
24. Use `None` and docstrings to specify dynamic default arguments
25. Enforce clarity with keyword-only and positional-only arguments
26. Define function decorators with `functools.wraps`

### Comprehensions and Generators
27. Use comprehensions instead of `map` and `filter`
``` python
a = [1,2,3,4,5]

# ok
alt = map(lambda x: x ** 2, a)

# better 
b = [x ** 2 for x in a]

# with filter
c = [x**2 for x in a if x % 2 == 0 ]

# dictionary comprehension
even_sq_dict = {x: x**2 for x in a if x % 2 == 0}

# set comprehension
three_cub_set = {x**3 for x in a if x % 3 == 0}
```
28. Avoid more than two control subexpressions in comprehensions
```python
# comprehensions support multiple levels of looping
matrix = [[1,2,3], [4,5,6], [7,8,9]]
flat = [x for row in matrix for x in row]
squared = [[x**2 for x in row] for row in matrix]

```
29. Avoid repeated work in comprehensions by using assignment expressions
``` python

# dict comprehesions and walrus operator
found = {name: batches for name in order
        if (batches := get_batches(stock.get(name, 0), 8))}

# note: the if statement is evaluated first in the comprehension

# if the walrus operator is in the comprehension part of the loop, then it leaks into containing scope
half = [(last := count // 2) for count in stock.values()] 
print(f'last item of {half} is {last}') # last = half[-1]

# doesn't leak value of count to containing scope
half = [count // 2 for count in stock.values()]

```

30. Consider using generators instead of returning lists
``` python
# why? avoids keeping the entire list in memory 
def index_file(handle): 
    offset = 0
    for line in handle:
        if line:
            yield offset
        for letter in line:
            offset += 1 
            if letter = ' ':
                yield offset


with open(path) as f:
    it = index_file(f)
    results = itertools.islice(it, 0, 10) 
    # results is a list of length 10
```

31. Be defensive when iterating over arguments
32. Consider generator expressions for large list comphrensions
33. Compose multiple generators with `yield from`
34. Avoid injecting data into generators with `send`
35. Avoid causin state transitions in generators with throw

### Classes and Interfaces
37. Compose classess instead of nesting many levels of built-in types
38. Accept functinos instead of classes for simple interfaces
39. Use `@classmethod` polymorphism to construct objects generically
40. Initialize parent classes with `super`
41. Consider composing functionality with mix-in classes
42. Prefer public attributes over private ones
43. Inherit from collections.abc for custome container types

### Metaclasses and Attributes
44. Use plain attributes instead of setter and getter methods
45. Consider `@property` instead of refactoring attributes
46. Use descriptors for resuable `@property` methods
47. Use `__getattr__`, `__getattribute__`, and `__setattr` for lazy attributes
48. Validate subclasses with `__init_subclass__`
49. Register class existance wiht `__set_name__`
50. Annotate class attributes with `__set_name__`
51. Prefer class decorators over metaclasses for composable class extensions

### Concurrency and Parallelism
52. Use `subprocess` to manage child processes

### Robustness and Performance

### Testing and Debugging

### Collaboration