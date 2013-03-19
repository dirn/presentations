.. title: NYC Python: MongoDB

:css: css/presentation.css

----

MongoDB Primer
==============

NYC Python

February 19, 2013

----

Things to know
==============

Collections, not tables

Documents, not rows

----

Things to know
==============

Versions
++++++++

Odd = development, even = production

2.3 will turn into 2.4

----

Things to know
==============

32-bit vs 64-bit?
+++++++++++++++++

64-bit. Always.

----

What is MongoDB?
================

To really understand what MongoDB is, you need to know what it is not

----

What is it not?
===============

Relational
++++++++++

No joins

----

What is it not?
===============

Relational
++++++++++

Joins at the application layer

----

What is it not?
===============

Relational
++++++++++

Embedded documents

----

What is it not?
===============

Transactional
+++++++++++++

No transactions

----

What is it not?
===============

Transactional
+++++++++++++

Transactions at the application layer

----

So what is it?
==============

Dynamic
+++++++

Schema is stored at the document level

----

So what is it?
==============

Scalable
++++++++

Replica sets and arbiters

Sharding

----

So what is it?
==============

Fast
++++

In memory

----

What you need to know about indexes
===================================

Directional
+++++++++++

.. code-block:: javascript

    db.collection.ensureIndex({field: 1})

    db.collection.ensureIndex({field: -1})

    db.collection.ensureIndex({field1: 1, field2: -1})

----

``explain()``

.. code-block:: javascript

    {
      "cursor" : "<Cursor Type and Index>",
      "isMultiKey" : <boolean>,
      "n" : <num>,
      "nscannedObjects" : <num>,
      "nscanned" : <num>,
      "nscannedObjectsAllPlans" : <num>,
      "nscannedAllPlans" : <num>,
      "scanAndOrder" : <boolean>,
      "indexOnly" : <boolean>,
      "nYields" : <num>,
      "nChunkSkips" : <num>,
      "millis" : <num>,
      "indexBounds" : { <index bounds> },
      "allPlans" : [
                     { "cursor" : "<Cursor Type and Index>",
                       "n" : <num>,
                       "nscannedObjects" : <num>,
                       "nscanned" : <num>,
                       "indexBounds" : { <index bounds> }
                     },
                      ...
                   ],
      "oldPlan" : {
                    "cursor" : "<Cursor Type and Index>",
                    "indexBounds" : { <index bounds> }
                  }
      "server" : "<host:port>",
    }

----

http://docs.mongodb.org/manual/reference/explain/#explain-output-fields-core

----

Atomic updates
==============

* ``$inc``
* ``$rename``
* ``$set``
* ``$unset``

----

Atomic updates
==============

* ``$pop``
* ``$pull``
* ``$pullAll``
* ``$addToSet`` / ``$each``
* ``$push``
* ``$pushAll``

----

http://docs.mongodb.org/manual/reference/operator/

----

Uses for collections
====================

*To the whiteboard...*
