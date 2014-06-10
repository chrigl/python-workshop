Iteratoren
==========

Niemals in den c-style mit incrementor-Variable verfallen
Iteratoren Benutzen!

.. code-block:: python

   >>> l = [1,2,3,4]

   >>> for elem in l:
   ...     print(elem)
   ...
   1
   2
   3
   4

Falls man einen Counter benötigt.

.. code-block:: python

   >>> for i, elem in enumerate(l):
   ...     print('%s: %s' % (i, elem))
   ...
   0: 1
   1: 2
   2: 3
   3: 4

Wie wird etwas iterierbar?
Oder... wie mache ich meine Klasse iterierbar?

Bsp. Stack, bzw. first in last out

.. code-block:: python

   >>> class Stack(object):
   ...     def __init__(self):
   ...         self._internal_stack = []
   ...     def push(self, value):
   ...         self._internal_stack.append(value)
   ...     def pop(self):
   ...         return self._internal_stack.pop()
   ...

   >>> stack = Stack()

   >>> stack.push(1)

   >>> stack.push(2)

   >>> stack.push(3)

   >>> stack.pop()
   3

   >>> stack.pop()
   2

   >>> stack.pop()
   1

   >>> stack.pop()
   ---------------------------------------------------------------------------
   IndexError                                Traceback (most recent call last)
   <ipython-input-38-50ea7ec13fbe> in <module>()
   ----> 1 stack.pop()

   <ipython-input-30-e086572bebe6> in pop(self)
         5         self._internal_stack.append(value)
         6     def pop(self):
   ----> 7         return self._internal_stack.pop()
         8

   >>> pop from empty list

Wie kann automatisch über die Liste gelaufen werden?
Ziel:

.. code-block:: python

   >>> for elem in stack:
   ...     print(elem)

Ein Iterator muss die Funktion __next__ implementieren
python3 __next__
python2 next

.. code-block:: python

   >>> class Stack(Iterator):
   ...     def __init__(self):
   ...         self._internal_stack = []
   ...     def push(self, value):
   ...         self._internal_stack.append(value)
   ...     def pop(self):
   ...         return self._internal_stack.pop()
   ...     def __next__(self):
   ...         if not self._internal_stack:
   ...             raise StopIteration
   ...         return self._internal_stack.pop()
   ...

   >>> stack = Stack()

   >>> stack.push(1)

   >>> stack.push(2)

   >>> for elem in stack:
   ...     print(elem)
   ...
   2
   1

Oder man implementiert __iter__

.. code-block:: python

   >>> del Stack.__next__

   >>> def __iter__(self):
   ...     for elem in reversed(self._internal_stack):
   ...         yield elem
   ...

   >>> stack = Stack()

   >>> Stack.__iter__ = __iter__

   >>> stack = Stack()

   >>> stack.push(1)

   >>> stack.push(2)

   >>> for elem in stack:
   ...     print(elem)
   ...
   2
   1

oder, weil reversed schon einen iterator zurück gibt

.. code-block:: python

   >>> def __iter__(self):
   ...     return reversed(self._internal_stack)
   ...

   >>> Stack.__iter__ = __iter__

   >>> stack = Stack()

   >>> stack.push(1)

   >>> stack.push(2)                                                             

   >>> for elem in stack:
   ...     print(elem)
   ...
   2
   1

ab python 3.4 yield from
um seine intention in code auszudrücken

.. code-block:: python

   >>> def __iter__(self):
   ...     yield from reversed(self._internal_stack)
   ...

   >>> Stack.__iter__ = __iter__

   >>> stack = Stack()

   >>> stack.push(1)

   >>> stack.push(2)

   >>> for elem in stack:
   ...     print(elem)
   ...
   2
   1

Siehe pyhton itertools
https://docs.python.org/2/library/itertools.html
