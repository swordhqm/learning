
```
That's because dir calls the __dir__ of the type of the input (equivalent to: type(inp).__dir__(inp)). For instances of a class it will call the classes __dir__ but if called on a class it will call the __dir__ of the meta-class.

class X(object):
    def __dir__(self):  # missing self parameter
        raise Exception("No!")

dir(X())  # instance!
# Exception: No!
So if you want to customize dir for your class (not instances of your class) you need to add a metaclass for your X:

import six

class DirMeta(type):
    def __dir__(cls):
        raise Exception("No!")

@six.add_metaclass(DirMeta)
class X(object):
    pass

dir(X)
# Exception: No!
```


https://eli.thegreenplace.net/2011/08/14/python-metaclasses-by-example

http://blog.jobbole.com/21351/

https://blog.ionelmc.ro/2015/02/09/understanding-python-metaclasses/