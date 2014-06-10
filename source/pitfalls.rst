Übliche Fehler
==============

ACHTUNG: intern Zeiger statt neuer Speicher

.. code-block:: python

   >>> ds = d

   >>> ds
   {'testing': 'foo', 'blah': 'xxx'}

   >>> ds['some'] = 'more'

   >>> ds
   {'testing': 'foo', 'some': 'more', 'blah': 'xxx'}

   >>> d
   {'testing': 'foo', 'some': 'more', 'blah': 'xxx'}

   >>> id(d)
   139694086836296

   >>> id(ds)
   139694086836296

... aber für den Fall gibt es copy

.. code-block:: python

   >>> import copy

   >>> dn = copy.copy(d)

   >>> id(dn)
   139694100470728

   >>> dn['foo'] = 'bar'

   >>> dn
   {'testing': 'foo', 'some': 'more', 'foo': 'bar', 'blah': 'xxx'}

   >>> d
   {'testing': 'foo', 'some': 'more', 'blah': 'xxx'}

Problem taucht bei Anfängern häufig bei Klassen auf.
Man kann Properties außerhalb des Konstruktors vordefinieren, sollte
sich dies aber überlegen.
... denn

.. code-block:: python

   >>> class Foo(object):
   ...     prop = []
   ...     def add(self, value):
   ...         self.prop.append(value)
   ...

   >>> foo1 = Foo()

   >>> foo2 = Foo()

   >>> foo1.add('Hello')

   >>> foo2.prop
   ['Hello']


Dieses Verhalten ist nur in den aller seltensten Fällen gewünscht und
verursacht in den meisten Fällen nur extremen Debugschmerz

Richtig wäre die Definition im Konstruktor

.. code-block:: python

   >>> class Foo(object):
   ...     def __init__(self):
   ...         self.prop = []
   ...     def add(self, value):
   ...         self.prop.append(value)
   ...

   >>> foo1 = Foo()

   >>> foo2 = Foo()

   >>> foo1.add('Hello')

   >>> foo2.prop
   []

   >>> foo1.prop
   ['Hello']
