:title: An Introduction to mock
:css: hovercraft.css
:skip-help: true

----

An Introduction to mock
=======================

A talk by `@dirn <https://twitter.com/dirn>`_

----

What is ``mock``?

----

`mock <http://mock.rtfd.org>`_ is a library created by
`Michael Foord @voidspace <https://twitter.com/voidspace>`_ that introduces
*fakes* to your tests.

----

Okay, so what's a fake?

----

A *fake* is an object used in tests to take the place of the real code.

----

**But I thought this was about something called mock**

----

A *mock* is a fake that you use to make assertions about how something is
called.

----

There's another kind of fake called a stub. A *stub* is a fake that you use to
make sure your code handles returned data correctly.

----

Why would anyone want to use fakes?

----

Fakes allow you to isolate the code you are trying to test from other parts of
your application.

*e.g., databases, APIs*

----

How do you use ``mock``?

----

.. code:: python

    from unittest import mock

----

Disclaimer
==========

I use Python 3

----

What's that? You're using Python 2?

----

**(╯°□°)╯︵ ┻━┻**

----

Fear not!

----

:class: has-code

.. code::

    $ pip install mock

.. code:: python

    import mock

----

Okay, but what do you do with it?

----

**Assert things**

----

Disclaimer #2
=============

The code that follows is oversimplified to demonstrate how easy ``mock`` is. You
wouldn't actually want to use it.

----

*Seriously, don't use this code!* [*]_

.. [*] Copyright (c) 2013, `James Powell`_

.. _James Powell: http://seriously.dontusethiscode.com

----

:class: has-code

.. code:: python

    def copy_dictionary(my_dict):
        return my_dict.copy()

    real_dict = {'a': 1, 'b': 2}
    copied_dict = copy_dictionary(real_dict)

    # Make sure the objects aren't the same
    assert copied_dict is not real_dict

    # Make sure the keys and values are
    assert set(real_dict.keys()) == set(copied_dict.keys())
    for k, v in real_dict.items():
        assert copied_dict[k] == v

----

With ``mock``

----

:class: has-code

.. code:: python

    def copy_dictionary(my_dict):
        return my_dict.copy()

    mock_dict = mock.Mock()
    copy_dictionary(mock_dict)

    # Make sure copy() was called
    mock_dict.copy.assert_was_called_once_with()

----

Another example

----

:class: has-code

.. code:: python

    def split_string(my_str, token=' '):
        return my_str.split(token)

    real_str = 'a b c'
    split_str = split_string(real_str)
    split_str2 = split_string(real_str, 'b')

    assert split_str == ['a', 'b', 'c']
    assert split_str2 == ['a ', ' c']

----

Again, with ``mock``

----

:class: has-code

.. code:: python

    def split_string(my_str, token=' '):
        return my_str.split(token)

    mock_str = mock.Mock()
    split_string(mock_str)
    split_string(mock_str, 'b')

    mock_str.split.assert_any_call(' ')
    mock_str.split.assert_called_with('b')

----

Now with named arguments

----

:class: has-code

.. code:: python

    def split_string(my_str, token=' '):
        return my_str.split(sep=token)

    mock_str = mock.Mock()
    split_string(mock_str)
    split_string(mock_str, 'b')

    mock_str.split.assert_any_call(sep=' ')
    mock_str.split.assert_called_with(sep='b')

----

**Replace things**

----

:class: has-code

.. code:: python

    class MyClass(object):
        my_attr = 'value'

    MockClass = mock.Mock()
    assert not hasattr(MockClass, 'not_my_attr'), 'uh oh'

----

.. code:: python

    AssertionError: uh oh

----

How do we make a ``Mock`` not have every attribute ever?

¯\\_(ツ)_/¯

----

Specifications to the rescue

----

:class: has-code

.. code:: python

    class MyClass(object):
        my_attr = 'value'

    MockClass = mock.Mock(spec=MyClass)
    assert not hasattr(MockClass, 'not_my_attr'), '\O/'

----

**Return things**

----

:class: has-code

.. code:: python

    def add_numbers(a, b):
        return a + b

    def add_all_numbers(a, b, c):
        return a + add_numbers(b, c)

    assert add_numbers(1, 2) == 3

    add_numbers = mock.Mock()
    add_numbers.return_value = 3

    assert add_all_numbers(3, 4, 5) == 6
    add_numbers.assert_called_once_with(4, 5)

----

But wait, there's more!

----

*Patching*
==========

``mock``'s secret sauce

----

What's a patch?
===============

``patch()`` can be used as a decorator or context manager to change something
within the current scope.

----

:class: has-code

.. code:: python

    def return_4():
        return 4

    with mock.patch('__main__.return_4') as return_5:
        return_5.return_value = 5
        assert return_4() == 5

    assert return_4() == 4

----

You can also patch objects

----

:class: has-code

.. code:: python

    class MyClass(object):
        def my_method(self):
            raise ValueError()

    with mock.patch.object(MyClass, 'my_method') as my_method:
        my_method.side_effect = TypeError
        try:
            MyClass().my_method()
        except TypeError:
            pass
        else:
            assert None

    try:
        MyClass().my_method()
    except ValueError:
        pass
    else:
        assert None

----

*Questions?*
