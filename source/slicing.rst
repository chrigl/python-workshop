Slicing
=======

TODO

Slicing bedeutet nur auf bestimmte Teile einer Liste zuzugreifen:

.. code-block:: python

   >>> l = [1,2,3,4,5]

Genau das 0. Element

.. code-block:: python

   >>> l[0]
   1

Alles ab dem 1. Element

.. code-block:: python

   >>> l[1:]
   [2, 3, 4, 5]

Alles bis zum 2. Element

.. code-block:: python

   >>> l[:2]
   [1, 2]

Ab dem 2. bis zum 5. 

.. code-block:: python

   >>> l[2:5]
   [3, 4, 5]

Das letzte Element

.. code-block:: python

   >>> l[-1]
   5

Ab dem 2. bis zum vorletzten Element

.. code-block:: python

   >>> l[2:-1]
   [3, 4]

An die Handhabung des 2. Parameters muss man sich einfach gewöhnen


itertools bietet wieder Hilfen um mit Iteratoren über Teillisten zu
arbeiten. z.B.

.. code-block:: python

   >>> from itertools import islice
   >>> gen = islice(l, 1, None)

   >>> [x for x in gen]
   [2, 3, 4, 5]

   >>> gen = islice(l, 1, 5)

   >>> [x for x in gen]
   [2, 3, 4, 5]
