Generatoren
===========

Für Iterationen über Listen sollten Generatoren verwendet werden
keyword yield

.. code-block:: python

   >>> def multiplier(in_):
   ...     for elem in in_:
   ...         yield elem * 2
   ...

   >>> gen = multiplier([1,2,3,4])

   >>> list(gen)
   [2, 4, 6, 8]

What?
for ruft automatisch immer next auf
bricht ab wenn eine StopIteration geworfen wird

.. code-block:: python

   >>> gen = multiplier([1,2,3,4])

   >>> next(gen)
   2

   >>> next(gen)
   4

   >>> next(gen)
   6

   >>> next(gen)
   8

   >>> next(gen)
   ---------------------------------------------------------------------------
   StopIteration                             Traceback (most recent call last)
   <ipython-input-9-8a6233884a6c> in <module>()
   ----> 1 next(gen)
   StopIteration:

Warum?
z.B. Chaining

.. code-block:: python

   >>> def adder(in_):
   ...     for elem in in_:
   ...         yield elem + 1
   ...

   >>> gen = adder(multiplier([1,2,3,4]))

   >>> list(gen)
   [3, 5, 7, 9]

oder auch:

.. code-block:: python

   >>> mul_gen = multiplier([1,2,3,4])

   >>> gen = adder(mul_gen)

   >>> list(gen)
   [3, 5, 7, 9]

Die Eingangsliste ist somit nur einmal ganz am Anfang und ggf. einmal ganz
am Ende im Speicher.
##
Oft bleibt der code dadurch einfacher
##
Einfache Erweiterbarkeit indem ein weiterer Generator dazwischen geschaltet
wird.

Ein kleiner Geschwindigkeitsvergleich:

.. code-block:: python

   >>> def naiv_adder(in_):
   ...     ret = []
   ...     for elem in in_:
   ...         ret.append(elem + 1)
   ...     return ret
   ...

   >>> def naiv_multiplier(in_):
   ...     ret = []
   ...     for elem in in_:
   ...         ret.append(elem * 2)
   ...     return ret
   ...

   >>> long_list = list(range(10000000))

   >>> %timeit ret = naiv_adder(naiv_multiplier(long_list))
   1 loops, best of 3: 2.83 s per loop

Doing it right:

.. code-block:: python

   >>> def adder(in_):
   ...     for elem in in_:
   ...         yield elem + 1
   ...

   >>> def multiplier(in_):
   ...     for elem in in_:
   ...         yield elem * 2
   ...

   # Funktioniert nur in ipython
   >>> %timeit ret = list(adder(multiplier(long_list)))
   1 loops, best of 3: 1.86 s per loop
