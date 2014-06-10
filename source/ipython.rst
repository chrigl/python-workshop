IPython
=======

Der fortschrittliche interaktive Interpreter IPython_.

.. code-block:: text

   $ ## Zuerst richtiges environment auswählen!
   $ pip install ipython
   $ ipython

.. code-block:: python

   In [1]: %quickref

Wichtiger built-in Datentyp list

.. code-block:: python

   In [2]: l = []
   
Standard python Hilfe

.. code-block:: python

   In [3]: help(l)

Man kann auch Methoden abfragen

.. code-block:: python

   In [4]: help(l.append)
   
ipython kann auch helfen

.. code-block:: python

   In [5]: l??
   Type:        list
   String form: []
   Length:      0
   Docstring:
   list() -> new empty list
   list(iterable) -> new list initialized from iterable's items

und ebenfalls Methoden abfragen

.. code-block:: python

   In [6]: l.append??
   Type:        builtin_function_or_method
   String form: <built-in method append of list object at 0x7f0d106b3048>
   Docstring:   L.append(object) -> None -- append object to end
   
Direktes editieren aus ipython heraus

.. code-block:: python

   In [7]: import os
   In [8]: %edit os
   
Weiterer wichtiger built-in Datentyp dict

.. code-block:: python

   In [9]: d = {'Hello': 'World'}
   
   In [10]: d.keys()
   Out[10]: dict_keys(['Hello'])
   
   In [11]: d.values()
   Out[11]: dict_values(['World'])
   
   In [12]: d['Hello']
   Out[12]: 'World'
   
   In [17]: d
   Out[17]: {'testing': 'foo', 'Hello': 'World', 'blah': 'xxx'}
   
   In [18]: del d['Hello']
   
   In [19]: d
   Out[19]: {'testing': 'foo', 'blah': 'xxx'}

.. _IPython: http://ipython.org/
