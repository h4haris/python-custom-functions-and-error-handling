# Python - Custom Functions and Error Handling (Data Science Toolbox)
Python function writing experimentation for Data Science
 
- To create custom functions, with multiple parameters and multiple return values, along with default arguments and variable-length arguments, 
- Scoping in Python, 
- Creating lambda functions and 
- Handling errors in function.

## Writing your own functions

### User-defined functions

```bash
1. Calling Built-in function
============================
# to covert object (such as int) to string
x = str(5)
Output: '5'

2. Defining & Calling Custom function (without parameters)
==========================================================
def square ():             # <- Function header
    new_value = 4 ** 2     # <- Function body
    print(new_value)

square()

Output: 16

3. Defining & Calling Custom function (with parameters)
=======================================================
def square (value):            
    new_value = value ** 2     
    print(new_value)

square(5)

Output: 25

4. Return values from functions
===============================
def square (value):            
    new_value = value ** 2     
    return new_value

num = square(2)
print(num)

Output: 4

5. With Docstrings
==================
def square (value):
    """Return the square of a value"""            
    new_value = value ** 2     
    return new_value
```

Parameters vs Arguments: 
- when we define a function, we write parameters in the function header. 
- when we call a function, we pass arguments into the function.

Docstrings
- describe what function does
- serve as documentation for function
- placed in the immediate line after the function header
- in between the triple double quotes """

**Note** *if there is no return type then type() will return NoneType*

### Multiple parameters and return values

Tuples
- like a list - can contain multiple values
- immutable - can't modify values
- constructed using parenthesis ()

```bash
1. Accessing tuple elements
===========================
a = even_nums[1]
b = even_nums[2]
c = even_nums[3]

2. Unpacking a tuple (in one line)
==================================
even_nums = (2, 4, 6)
a, b, c = even_nums
```

```bash
1. Function with multiple parameters
====================================
def raise_to_power (value1, value2):            
    """Raise value1 to the power of value2."""            
    new_value = value1 ** value2     
    return new_value

result = raise_to_power(2,3)
print(result)

Output: 8

2. Function with multiple return values
=======================================
# return in tuples using ()

def raise_both (value1, value2):            
    """Raise value1 to the power of value2 and vice versa."""            
    new_value1 = value1 ** value2     
    new_value2 = value2 ** value1

    new_tuple = (new_value1, new_value2)     
    return new_tuple

result = raise_both(2,3)
print(result)

Output: (8,9)
```

## Default arguments, variable-length arguments and scope

### Scope and user-defined functions

#### Scope
- Not all objects are accessible everywhere in a script
- Scope - part of the program where an object or name may be accessible
    - Global scope - defined in the main body of a script
    - Local scope - defined inside a function
    - Built-in scope - names in the pre-defined built-ins module

**when we reference a name, first the local scope is searched, then the global. If the name is in neither, then the built-in scope is searched.**

```bash
Alter global scope variable in local scope
==========================================
new_val = 10

def square(value):
    """Returns the square of a number."""
    global new_val
    new_val = new_val ** 2
    return new_val
square(3)

Output: 100

new_val

Output: 100
```

#### Check builtin functions

check Python's built-in scope, which is just a built-in module called builtins. 

```bash
# print a list of all the names in the module builtins.
import builtins
dir(builtins)
```
### Nested functions

```bash
1. Simple Function
==================
def mod2plus5(x1, x2, x3):
    """Returns the remainder plus 5 of three values."""

    new_x1 = x1 % 2 + 5
    new_x2 = x2 % 2 + 5
    new_x3 = x3 % 2 + 5

    return (new_x1, new_x2, new_x3)

mod2plus5(1,2,3)

Output: (6,5,6)

2. Converted to Nested Function
===============================
def mod2plus5(x1, x2, x3):
    """Returns the remainder plus 5 of three values."""

    def inner(x):
        """Returns the remainder plus 5 of a value."""
        return = x % 2 + 5

    return (inner(x1), inner(x2), inner(x3))

mod2plus5(1,2,3)

Output: (6,5,6)

3. Returning functions
======================
def raise_val(n):
    """Return the inner function."""

    def inner(x):
        """Raise x to the power of n."""
        raised = x ** n
        return raised

    return inner

square = raise_val(2)
cube = raise_val(3)
print (square(2), cube(4))

Output: 4,64

# when we call the function square, it remembers the value n=2, although the enclosing scope defined by raise_val and to which n=2 is local, has finished execution. 
# This subtlety referred to as a closure - means that the nested or inner function remembers the state of its enclosing scope when called

4. Using nonlocal to alter enclosing scope variable (parent function variable)
==============================================================================
def outer():
    """Print the value of n."""
    n = 1

    def inner():
        nonlocal n
        n = 2
        print(n)

    inner()
    print(n)

outer()

Output: 2
        2
```

Scope Ordering LEGB
- L Local
- E Enclosing functions
- G Global
- B Built-in


### Default and flexible arguments

```bash
1. Default functions
====================
def power(number, pow=1):
    """Raise number to power of pow."""
    new_value = number ** pow

    return new_value

power(9,2) => Output: 81
power(9) => Output: 9

2. Flexible arguments (*args)
=============================
# good when we aren't sure how many arguments a user will want to pass
# Here function can takes floats or ints
# args passed as tuple in function body
def add_all(*args):
    """Sum all values in xargs together."""
    # Initialize sum
    sum_all = 0
    # Accumulate the sum
    for num in args:
        sum_all += num

    return sum_all


add_all(1) => Output: 1
add_all(5,10,15) => Output: 30


3. Flexible arguments (**kwargs)
================================
# use a double star to pass an arbitrary number of keyword arguments, called kwargs
# i.e. arguments preceded by identifiers/key
# turns the identifier-keyword pairs into a dictionary within the function body
def print_all(**kwargs):
    """Print out key-value pairs in **kwargs."""

    # Print out the key-value pairs
    for key, value in kwargs.items():
        print(key + ": " + value)


print_all(name="dumbledore", job="headmaster") 
Output: job: headmaster
        name: dumbledore
```

**Note** *that it is NOT the names args and kwargs that are important when using flexible arguments, but rather that they're preceded by a single and double star, respectively.*

## Lambda functions and error-handling

### Lambda functions

We can define by using keyword **lambda**
Come in handy in many situations, e.g. 
- Anonymous functions
    - function map takes 2 arguments: map(func, seq)
    - map() applies the function to all elements in the sequence
    - We can pass lambda functions to map without even naming them and by referring them as **anonymous functions**

```bash
1. Lambda functions
===================
raise_to_power = lambda x, y: x ** y

raise_to_power(2, 3)

Output: 8

2. Anonymous functions
======================
nums = [48, 6, 9, 21, 1]

square_all = map(lambda num: num ** 2, nums)

print(square_all)       => Output: <map object
print(list(square_all)) => Output: [2304, 36, 81, 441, 1]
```

### Error Handling

```bash
1. Errors and exceptions
========================
def sqrt(x):
    "Returns the square root of a number."
    try:
        return x ** 0.5
    except:
        print('x must be an int or float')

sqrt(4)     => Output: 2.0
sqrt('hi')  => Output: x must be an int or float

2. Catching only TypeError
==========================
def sqrt(x):
    """Returns the square root of a number."""
    try:
        return x ** 0.5
    except TypeError:
        print('x must be an int or float')

3. Raising an error
===================
def sqrt(x):
    """Returns the square root of a number."""
    if x < 0:
        raise ValueError('x must be non-negative')
    try:
        return x ** 0.5
    except TypeError:
        print('x must be an int or float')

sqrt(-1)     => Output: ValueError: x must be non-negative
```