:title: An Introduction to mock
:css: hovercraft.css
:skip-help: true

----

An Introduction to mock
=======================

A talk by `@dirn <http://twitter.com/dirn>`_

----

What is mock?
=============

*mock* is a library that introduces *fakes* to your tests.

----

Okay, so what's a fake?
=======================

A *fake* is an object used in tests to take the place of the real code.

----

Two kinds of fakes
==================

1. A *mock* is a fake that you use to make assertions about how something is
   called.

2. A *stub* is a fake that you use to make sure your code handles returned
   data correctly.

----

:class: has-code

How does mock work?
===================

.. code:: python

    from unittest import mock

----

Using Python 2?
===============

(╯°□°)╯︵ ┻━┻

----

:class: has-code

There's a Pip for that
======================

.. code::

    $ pip install mock

.. code:: python

    import mock

----

Okay, but how do I use it?

----

:class: has-code

Replacing objects
=================

.. code:: python

    class MyClass(object):
        my_attr = 'value'

    MockClass = mock.Mock()
    assert not hasattr(MockClass, 'not_my_attr'), 'this will fail'

----

:class: has-code

Replacing objects
=================

.. code:: python

    class MyClass(object):
        my_attr = 'value'

    MockClass = mock.Mock(spec=MyClass)
    assert not hasattr(MockClass, 'not_my_attr'), 'this will pass'

----

:class: has-code

Making assertions
=================

.. code:: python

    def copy_dictionary(my_dict):
        return my_dict.copy()

    real_dict = {'a': 1, 'b': 2}
    copied_dict = copy_dictionary(real_dict)
    assert copied_dict is not real_dict
    assert set(real_dict.keys()) == set(copied_dict.keys())
    for k, v in real_dict.items():
        assert copied_dict[k] == v

----

:class: has-code

Making assertions
=================

.. code:: python

    def copy_dictionary(my_dict):
        return my_dict.copy()

    mock_dict = mock.Mock()
    copy_dictionary(mock_dict)

    mock_dict.copy.assert_was_called_once_with()

----

:class: has-code

Making assertions
=================

.. code:: python

    def split_string(my_str, token=' '):
        return my_str.split(token)

    real_str = 'a b c'
    split_str = split_string(real_str)
    split_str2 = split_string(real_str, 'b')

    assert split_str == ['a', 'b', 'c']
    assert split_str2 == ['a ', ' c']

----

:class: has-code

Making assertions
=================

.. code:: python

    def split_string(my_str, token=' '):
        return my_str.split(token)

    mock_str = mock.Mock()
    split_string(mock_str)
    split_string(mock_str, 'b')

    mock_str.split.assert_any_call(' ')
    mock_str.split.assert_called_with('b')

----

:class: has-code

Making assertions
=================

.. code:: python

    def split_string(my_str, token=' '):
        return my_str.split(sep=token)

    mock_str = mock.Mock()
    split_string(mock_str)
    split_string(mock_str, 'b')

    mock_str.split.assert_any_call(sep=' ')
    mock_str.split.assert_called_with(sep='b')

----

But wait, there's more!

----

*Patching*
==========

mock's secret sauce

----

What's a patch?
===============

``patch()`` can be used as a decorator or context manager to your target within
the current scope.
