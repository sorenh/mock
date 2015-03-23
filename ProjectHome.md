Download the latest release of mock from the [PyPI page](http://pypi.python.org/pypi/mock).

For support and discussion, use the [Testing in Python](http://lists.idyll.org/listinfo/testing-in-python) mailing list. ([testing-in-python@lists.idyll.org](http://lists.idyll.org/listinfo/testing-in-python))

mock is a Python module that provides a core Mock class. It is intended to reduce the need for creating a host of trivial stubs throughout your test suite. After performing an action, you can make assertions about which methods / attributes were used and arguments they were called with. You can also specify return values and set needed attributes in the normal way.

It also provides utility functions / objects to assist with testing, particularly monkey patching.

mock is tested with Python 2.4 - 2.7, and Python 3.

Mock is very easy to use and is designed for use with [unittest](http://pypi.python.org/pypi/unittest2). Mock is based on the 'action -> assertion' pattern instead of 'record -> replay' used by many mocking frameworks. See the [mock documentation](http://www.voidspace.org.uk/python/mock) for full details.

Mock objects create all attributes and methods as you access them and store details of how they have been used. You can configure them, to specify return values or limit what attributes are available, and then make assertions about how they have been used.

```
>>> from mock import Mock
>>> real = ProductionClass()
>>> real.method = Mock()
>>> real.method.return_value = 3
>>>
>>> real.method(3, 4, 5, key='value')
3
>>> real.method.assert_called_with(3, 4, 5, key='value')
```

The `patch` decorator / context manager makes it easy to mock classes or objects in a module under test. The object you specify will be replaced with a mock (or other object) during the test and restored when the test ends:
```
@patch('test_module.ClassName1')
@patch('test_module.ClassName2')
def test_method(self, MockClass2, MockClass1):
    test_module.ClassName1()
    test_module.ClassName2()

    self.assertTrue(MockClass1.called, "ClassName1 not patched")
    self.assertTrue(MockClass2.called, "ClassName2 not patched")
```

mock 0.7.0 supports the mocking of magic methods and also works with Python 3. The easiest way of using magic methods is with the MagicMock class. It allows you to do things like:

```
>>> from mock import MagicMock
>>> mock = MagicMock()
>>> mock.__str__.return_value = 'foobarbaz'
>>> str(mock)
'foobarbaz'
>>> mock.__str__.assert_called_with()
```

In 0.7 Mock allows you to assign functions (or other Mock instances) to magic methods and they will be called appropriately. The MagicMock class is just a Mock variant that has all of the magic methods pre-created for you (well - all the useful ones anyway).

The following is an example of using magic methods with the ordinary Mock class:
```
>>> from mock import Mock
>>> mock = Mock()
>>> mock.__str__ = Mock()
>>> mock.__str__.return_value = 'wheeeeee'
>>> str(mock)
'wheeeeee'
```


There is also `patch.dict` for setting values in a dictionary just during the
scope of a test and restoring the dictionary to its original state when the
test ends:
```
   >>> foo = {'key': 'value'}
   >>> original = foo.copy()
   >>> with patch.dict(foo, {'newkey': 'newvalue'}, clear=True):
   ...     assert foo == {'newkey': 'newvalue'}
   ...
   >>> assert foo == original
```