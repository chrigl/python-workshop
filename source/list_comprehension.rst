List Comprehensions
===================

List Comprehensions machen die Arbeiten mit Listen einfacher

Beispiel naiv ohne List Comprehensions

.. code-block:: python

   >>> l = [1,2,3,4,5]

   >>> ret = []

   >>> for elem in l:
   ...     if elem % 2 == 0:
   ...         ret.append(elem)
   ...

   >>> ret
   [2, 4]

Oder das Selbe als List Comprehension

.. code-block:: python

   >>> ret = [elem for elem in l if elem % 2 == 0]

   >>> ret
   [2, 4]

Die Benutzung von List Comprehensions machen den Code oft weniger anfällig
für Fehler.
Oft führt die Benutzung auch zu saubererem Code.

.. code-block:: python

   >>> l = [{'data': 'some data1'},
             {'data': 'some data2'},
             {'data': 'some data3'}]

Zu jedem Dictionary innerhalb dieser Liste soll ein Feld mit dem Namen
sequenceno hinzugefügt werden...
[{'data': 'some data1', 'sequenceno': 1}, ...}

.. code-block:: python

   >>> l = [{'data': 'some data1'}, {'data': 'some data2'}, {'data': 'some data3'}]

   >>> for no, elem in enumerate(l):
   ...     elem['sequenceno'] = no
   ...     ret.append(elem)
   ...

   >>> ret
   [{'sequenceno': 0, 'data': 'some data1'},
    {'sequenceno': 1, 'data': 'some data2'},
    {'sequenceno': 2, 'data': 'some data3'}]

Problem hier. Die Manipulation von elem passiert hier mittem im Code-Block
und ermöglicht keine Wiederverwendbarkeit des Codes.
Zugegeben, die "Arbeit" im Beispiel ist klein, wird aber bei großen
zusammengehörenden Blöcken um so wichtiger.


Im Endeffekt möchte man die Möglichkeit haben, ein Element mit einer
sequenceno zu versehen. Dieser Code würde sich an beliebigen Stellen
auf die Struktur ({'data': '...'}) verwenden lassen. Unabhängig davon
ob das Element gerade aus einer Liste kommt oder nicht.

.. code-block:: python

   >>> def _add_sequence_no(elem, no):
   ...     elem['sequenceno'] = no
   ...     return elem
   ...

   >>> ret = _add_sequence_no({'data': 'testing'}, 0)

   >>> ret
   {'sequenceno': 0, 'data': 'testing'}

_add_sequence_no läßt sich jetzt einfach in einer List Comprehension
verwenden.

.. code-block:: python

   >>> l = [{'data': 'some data1'}, {'data': 'some data2'}, {'data': 'some data3'}]

   >>> ret = [_add_sequence_no(elem, no) for no, elem in enumerate(l)]

   >>> ret
   [{'sequenceno': 0, 'data': 'some data1'},
    {'sequenceno': 1, 'data': 'some data2'},
    {'sequenceno': 2, 'data': 'some data3'}]

VORSICHT... auch hier haben wir wieder das Zeiger-/Speicherbereichproblem

.. code-block:: python

   >>> l
   [{'sequenceno': 0, 'data': 'some data1'},
    {'sequenceno': 1, 'data': 'some data2'},
    {'sequenceno': 2, 'data': 'some data3'}]

Die Elemente in der Ausgangsliste wurden ebenfalls verändert, da sie als
Zeiger in _add_sequence_no geben wird.
Das Selbe passiert aber auch bei dem naiven Ansatz.
Hier kann wieder copy verwendet werden um kopien zu erzeugen.

.. code-block:: python

   >>> import copy

   >>> def _add_sequence_no(elem, no):
   ...     new_elem = copy.copy(elem)
   ...     new_elem['sequenceno'] = no
   ...     return new_elem
   ...

   >>> l = [{'data': 'some data1'}, {'data': 'some data2'}, {'data': 'some data3'}]

   >>> ret = [_add_sequence_no(elem, no) for no, elem in enumerate(l)]

   >>> ret
   [{'sequenceno': 0, 'data': 'some data1'},
    {'sequenceno': 1, 'data': 'some data2'},
    {'sequenceno': 2, 'data': 'some data3'}]

   >>> l
   [{'data': 'some data1'}, {'data': 'some data2'}, {'data': 'some data3'}]

l ist also hier unverändert.


Möchte man an Stelle von zurückgegebenen Listen lieber mit einem generator
arbeiten ist dies einfach durch das Benutzen von () möglich.

.. code-block:: python

   >>> gen = (_add_sequence_no(elem, no) for no, elem in enumerate(l))

   >>> gen
   <generator object <genexpr> at 0x7f3c644cff30>

Dieser erzeugte Generator kann wie gehabt verwendet werden.

z.B.

.. code-block:: python

   >>> next(gen)
   {'sequenceno': 0, 'data': 'some data1'}

   >>> next(gen)
   {'sequenceno': 1, 'data': 'some data2'}

   >>> next(gen)
   {'sequenceno': 2, 'data': 'some data3'}

   >>> next(gen)
   ---------------------------------------------------------------------------
   StopIteration                             Traceback (most recent call last)
   <ipython-input-44-8a6233884a6c> in <module>()
   ----> 1 next(gen)

   StopIteration:
