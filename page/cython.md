---
layout: page
title: "cython tutorial and transfer btstack lib as a pyd library"
description: ""
---
{% include JB/setup %}

### cython tutorial
[http://apprize.info/python/cython/index.html](http://apprize.info/python/cython/index.html)

[http://cython.readthedocs.io/en/latest/src/userguide/extension_types.html](http://cython.readthedocs.io/en/latest/src/userguide/extension_types.html)

### cython faq
[https://github.com/cython/cython/wiki/FAQ](https://github.com/cython/cython/wiki/FAQ)

### tutorial pdf
[https://media.readthedocs.org/pdf/cython/stable/cython.pdf](https://media.readthedocs.org/pdf/cython/stable/cython.pdf)

### Study Record
20170924 use callback sample to do a function table
1. use ctypedef to declear the prototype of a function or a type
```
cdef extern from "cheesefinder.h":
    ctypedef void (*cheesefunc)(char *name, void *user_data)
    ctypedef struct cheesetable:
        pass
```

2. global variable / struct global variable could be declear inside pyx file
  but you need to have the typedef first
```
cdef cheesefunc g_table3
```
  should have 'cheesetable' type , come from 'extern from a prototpe file'
```
cdef extern from "cheesefinder.h":
    ctypedef void (*cheesefunc)(char *name, void *user_data)
```

3. all code inside pyx are python protoype and have primitive of c-lang type.
  It only has 'char *', 'void *', 'int' for declear. 
  inside code body, it still uses python style and code. 
  you needs 'global <variable>' to access the global variable of python. 
```
cdef int mycalclen(char *name, void *f):
    global prefix  # for example: "value-len:"
    l = 0
    if name:
        l = len(name.decode('utf-8'))
    print("inside mycalclen " + prefix + str(l))
    return l
```

### Misc
1. reference count problem
2. string buffer example
3. lifetime example
    
