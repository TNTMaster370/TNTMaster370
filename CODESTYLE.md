## Introduction
This document details my personal standard for maintaining a code style convention. This particular
document will focus mainly on Python code styles. To be clear, I still maintain 
[PEP 8](https://peps.python.org/pep-0008/). That said, this document serves as an addition for 
convention outside of PEP 8.

This documents focuses primarily on how I prefer things to be ordered. That said, this is not a firm
ruleset.

#### Order of Functions Inside Class
When the body of the class is defined, the order of the functions may not necessarily matter. Unless
the order of the class parallels a parent class that is not under this style, it should be that the 
order follows a hierarchy of sorts:

- Any class attributes, including `__slots__`.
```
class A:
    __slots__ = ...

    some_class_attribute = ...
```

- Any double-underscore (dunder) methods, preferably sorted in a similar order to how the
[data model](https://docs.python.org/3/reference/datamodel.html) is ordered.
```
class B:
    def __init__(self):
        ...

    def __hash__(self):
        ...
```

- Any 'hidden' functions (name starts with a single underscore).
```
class C:
    def _hidden_function(self):
        # This function should not be called externally.
        ...
```

- Any other external function, preferring alphabetical sorting. (See the section "Order of Names")
```
class D:
    def some_function(self):
        ...
```

#### Order of Import Statements
When a large-scale project is being developed, it's only natural for some files to accumulate a 
large sum of import statements, importing many things to function. The imports may be difficult to 
sift through, and make it difficult to understand what names come from where. For this reason, this 
document suggests that each import be separated by an empty line by where it's obtained from. The 
order of these types are, in order of first declaration to last declaration:

- \_\_future\_\_ imports.
  - This is because Python mandates any \_\_future\_\_ imports come before any other action.
- Inter-project imports. (local)
  - This is because, if a circular import loop occurs, Python will have to do much less work to
  catch it.
- Built-in module imports.
- External imports (dependency projects, etc)

For example:
```
from __future__ import annotations  # __future__

from .some_local_module import some_function  # local

from typing import Any  # built-in

from attrs import ...  # external
```

It may be permitted to separate each external project with an empty line further, but this is not 
required.
```
from attrs import ...
# Presume there are five additional lines of imports from `attrs`

from tqdm import ...
```

#### Order of Names
When the names of any series of attributes, functions, and/or classes shouldn't matter, it can be 
difficult to know how to order them. It could be ordered arbitrarily (in order of implementation, 
for example), but that makes finding it later on challenging, especially for large files.

This section of the code style document suggests using a fixed sorting procedure to determine the 
ordering of some series of features.

The first part of it is to ensure that the ordering *truly* does not matter. It is important to ask 
questions with relation to it. For each feature:

- Does it depend on another feature? Does any other feature depend on it?
- Do the features have some implicit relationship (eg. __add__, __radd__)?
- Does it's functionality or type annotations rely on the creation of some global state?

If any of these questions suggest that the order does matter, the respective order that it suggests 
must take priority. For cases where the order should not matter, the solution should be that the 
features are sorted in alphabetical order.

```
# Sorted first because it comes first alphabetically.
def a():
    ...

# Sorted second because it comes second alphabetically.
class B:
    ...
```

In this example, the function `a` is before the class `B`, because of the alphabetical sorting. 
This allows the order to not truly matter, while maintaining easy searching. For 'hidden' features 
(features whose name starts with an underscore), it should come before all others. This is mainly 
because hidden features are mainly to be depended on, but in the case where its order doesn't 
matter with another feature that doesn't depend on it, the hidden feature should be treated as 
though it comes before the other alphabetically.

```
function _c():
    ...

function b():
    # Is not dependant on _c().
    ...
```

For the last case, in which two independent features have the same name with different 
capitalization, such `attribute` versus `Attribute`. When the order truly doesn't matter there, the 
lowercase version takes priority over the uppercase version.


