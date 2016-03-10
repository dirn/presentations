:title: Packaging Your Pythons
:css: hovercraft.css
:skip-help: true

----

Packaging Your Pythons
======================

A talk by `@dirn <https://twitter.com/dirn>`_

----

So you want to package your code
--------------------------------

----

Where do you begin?

----

``setup.py``

----

:class: has-code

.. code:: python

    from setuptools import setup

    setup(
        name='nycpython',
        version='1.0.0',
        description='The official package of NYC Python',
        url='https://github.com/NYCPython/nycpython',
        author='Andy Dirnberger',
        author_email='andy@nycpython.com',
        license='BSD',
        packages=['nycpython'],
        zip_safe=False,
    )

----

:data-rotate: 270

*An aside about versions*

----

`PEP 386 <http://www.python.org/dev/peps/pep-0386/>`_ (current)

`PEP 440 <http://www.python.org/dev/peps/pep-0440/>`_ (future)

----

:data-rotate: 0

Package layout

----

:class: has-code

.. code:: sh

    $ tree
    .
    |-- nycpython
    |   |-- __init__.py
    |-- setup.py

----

``nycpython/__init__.py``

----

:class: has-code

.. code:: python

    """The magic of NYC Python"""

    __version__ = '1.0.0'


    def make_the_awesome():
        return 'http://nycpython.com'

----

:class: has-code

.. code:: sh

    $ python

.. code:: python

    from nycpython import make_the_awesome
    print(make_the_awesome())

----

:data-rotate: 270

*An aside about modules*

----

instead of

``nycpython/__init__.py``

and

``packages=['nycpython']``

----

we could have used

``nycpython.py``

and

``pymodules=['nycpython']``


----

*Why didn't we?*

----

Growth

----

:data-rotate: 0

That was easy, right?

----

**But this is open source night**

we can do better

----

README
------

----

LICENSE
-------

----

:class: has-code

.. code:: sh

    $ tree
    .
    |-- LICENSE
    |-- nycpython
    |   |-- __init__.py
    |-- README.rst
    |-- setup.py

----

:class: has-code

.. code:: python

    from setuptools import setup

    from nycpython import __version__


    def read_file(filename):
        with open(filename) as f:
            return f.read()

    setup(
        name='nycpython',
        version=__version__,
        description='The official package of NYC Python',
        long_description=read_file('README.rst'),
        url='https://github.com/NYCPython/nycpython',
        author='Andy Dirnberger',
        author_email='andy@nycpython.com',
        license=read_file('LICENSE'),
        packages=['nycpython'],
        zip_safe=False,
        classifiers=[...],
    )

----

Learn from my mistake
---------------------

----

``MANIFEST.in``

----

``include LICENSE README.rst``

~ or ~

``include *.rst LICENSE``

----

:class: has-code

.. code:: sh

    $ tree
    .
    |-- LICENSE
    |-- MANIFEST.in
    |-- nycpython
    |   |-- __init__.py
    |-- README.rst
    |-- setup.py

----

:data-rotate: 270

*An aside about reStructured Text*

----

Why not Markdown?

----

PyPI [*]_

.. [*] more on that later

----

`Sphinx <http://sphinx.rtfd.org>`_

----

`Read the Docs <http://rtfd.org>`_

----

:data-rotate: 0

Publishing to PyPI
------------------

----

Now then, some cheese please, my good man.

*Certainly, sir. What would you like?*

Well, eh, how about a little red Leicester.

*I'm, afraid we're fresh out of red Leicester, sir.*

----

Register on the `Python Package Index <https://pypi.python.org/pypi>`_

----

**Not so fast!!**

----

Register on the `test site <https://testpypi.python.org/pypi>`_

----

``~/.pypirc``

----

:class: has-code

.. code:: ini

    [distutils]
    index-servers =
        pypi
        test

    [pypi]
    repository = http://pypi.python.org/pypi
    username = dirn
    password = <my super secret password goes here>

    [test]
    repository = https://testpypi.python.org/pypi
    username = dirn
    password = <another super secret password goes here>

----

Building and Uploading
----------------------

----

.. code:: sh

    $ python setup.py sdist

----

:data-rotate: 270

*An aside about testing your build*

----

:class: has-code

.. code:: sh

    $ rm -rf build-env
    $ virtualenv build-env
    $ build-env/bin/pip install --no-index dist/nycpython-1.0.0.tar.gz
    ...
    $ build-env/bin/python
    >>> import nycpython
    >>> nycpython.__version__
    '1.0.0'

----

:data-rotate: 0

:class: has-code

.. code:: sh

    $ python setup.py register -r test
    $ python setup.py sdist upload -r test

----

:class: has-code

.. code:: sh

    $ python setup.py register -r pypi
    $ python setup.py sdist upload -r pypi

----

Installation
------------

.. code:: sh

    $ pip install nycpython

----

:data-rotate: 270

*An aside about the future*

----

`Wheel <http://wheel.rtfd.org>`_ [*]_

.. [*] Because 'newegg' was taken.

----

``setup.cfg``

----

:class: has-code

.. code:: ini

    [wheel]
    universal = 1

----

:class: has-code

.. code:: sh

    $ pip install wheel
    $ python setup.py bdist_wheel

----

:class: has-code

.. code:: sh

    $ rm -rf build-env
    $ virtualenv build-env
    $ build-env/bin/pip install --use-wheel --no-index \
        --find-links dist nycpython
    ...
    $ build-env/bin/python
    >>> import nycpython
    >>> nycpython.__version__
    '1.0.0'

----

:class: has-code

.. code:: sh

    $ python setup.py bdist_wheel upload -r test

and

.. code:: sh

    $ python setup.py bdist_wheel upload -r pypi

----

:data-rotate: 0

*Questions?*
