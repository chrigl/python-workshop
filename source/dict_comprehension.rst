Dict Comprehension
==================

Neben List Comprehensions gibt es zum erzeugen von dictionaries seit
python 2.7 auch Dict Comprehensions

.. code-block:: python

   >>> l = ['a', 'b', 'c']

   >>> d = {v: k for k, v in enumerate(l)}

   >>> d
   {'c': 2, 'a': 0, 'b': 1}

Muss man auch python 2.6 unterstützen kann man für die selbe Aufgabe auf
einen Konstruktor von dict zurückgreifen, was auch in python3 noch genau
so funktioniert.

.. code-block:: python

   >>> d = dict((v, k) for k, v in enumerate(l))

   >>> d
   {'c': 2, 'a': 0, 'b': 1}
