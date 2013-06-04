:title: Know Your Tests
:css: hovercraft.css
:skip-help: true

----

Know Your Tests
===============

A lightning talk by `@dirn <http://twitter.com/dirn>`_

----

Types of Tests
==============

* Unit Tests
* Integration Tests
* Functional Tests

----

Types of Tests
==============

*Unit tests* test that units of your code work.

----

Types of Tests
==============

*Integration tests* test how pieces of the application integrate with each
other.

----

Types of Tests
==============

*Functional tests* test that your application actually functions.

----

:class: has-code

Example 1
=========

::

    def double(a):
        return a * 2

----

:class: has-code

Example 1
=========

::

    assert double(1) == 2
    assert double(2) == 4
    assert double(3) == 6

----

Example 1
=========

**Unit Test**

----

:class: has-code

Example 2
=========

::

    def get_github_id(username):
        url = 'https://api.github.com/users/{}'
        resp = requests.get(url.format(username))
        if resp.status_code != 200:
            return None
        return resp.json().get('id', None)

----

:class: has-code

Example 2
=========

::

    assert get_github_id('dirn') == 168109
    assert get_github_id('dutc') == 3922744
    assert get_github_id('Julian') == 329822

----

Example 2
=========

**Integration Test**

----

:class: has-code

Example 3
=========

::

    <h1>NYCPython GitHub Users</h1>

    <dl>
        <dt>dirn
        <dd>336218
        <dt>dutc
        <dd>7845488
        <dt>Julian
        <dd>659644
    </dl>

----

:class: has-code

Example 3
=========

::

    browser = webdriver.Chrome()
    browser.get('http://example.com')
    h1 = browser.find_elements_by_tag_name('h1')
    assert len(h1) == 1

----

:class: has-code

Example 3
=========

::

    users = browser.find_element_by_tag_name('dl')
    usernames = users.find_elements_by_tag_name('dt')
    doubles = users.find_elements_by_tag_name('dd')
    for user, id in zip(usernames, doubles):
        assert double(get_github_id(user)) == id

----

Example 3
=========

**Functional test**
