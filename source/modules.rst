Module
======

Was sind Module? Wofür benötigen wir Module? Wie erstelle ich ein Modul?

Was sind Module und wofür benötigen wir sie?
--------------------------------------------

Code, der in anderen Bereichen wieder verwendet werden kann, sollte in Module
gegliedert werden. Ein Modul kann dabei aus einer einzigen oder einem Satz von
Python-Dateien bestehen. Innerhalb der Module werden globale Variablen,
Klassen, Funktionen, etc. definiert. Diese können (falls sie sich im PYTHONPATH
befindet; mehr dazu später) von jedem Python-Script verwendet werden. Außerdem
macht es Sinn ein Modul für einen Zweck zu erstellen, wie z.B. das schon
benutzte Modul ``copy``.


Also... was macht einfache Module aus?
--------------------------------------

Im einfachsten Fall kann jede Python-Datei mit der Endung .py als Modul
verwendet werden. Verwenden bedeutet hier mittels ``import`` verwendet werden.


Wechseln wir z.B. in den Ordner ``testing`` und erzeugen eine Datei ``test.py``

.. code-block:: python

   def do_something(x):
       return x + 1

   def do_somthing_else(x):
       return x + 2

Starten wir im selben Verzeichnis den schon bekannten Interpreter

.. code-block:: python

   >>> from test import do_something
   >>> do_something(1)
   2
   >>> from test import do_something_else
   >>> do_something_else(1)
   3

Es kann also jetzt im selben Ordner eine Datei ``run.py`` erzeugt werden, die
die beiden Funktionen verwendet:

.. code-block:: python

   from test import do_something
   from test import do_something_else

   print("do_something: %s" % do_something(1))
   print("do_something_else: %s" % do_something_else(1))

.. code-block:: sh

   $ python run.py
   do_something: 2
   do_something_else: 3

Neben des ``from test import X`` kann auch nur ``import`` verwendet werden

.. code-block:: python

   >>> import test
   >>> test.do_something(1)
   2

Darf es auch ein wenig komplizierter werden?
--------------------------------------------

In den seltensten Fällen besteht ein Modul nur aus einer einzigen Datei. Oft
stellt man die eigentlich API zwar in einer Datei gesammelt zur Verfügung, die
eigentliche Implementation steckt aber in weiteren Dateien. Es macht z.B. Sinn
eigene Exceptions in eine eigene Datei zu packen. Zudem trennt man z.B.
logische dinge wie Encoder und Decoder in einzelne Dateien.


Als kleines Beispiel erstellen wir ein Modul ``abrechnung``. Diese Modul hat
eine Datenhalde (wir werden der einfachheit halber ein Dictionary verwenden).
Dort werden die einzelnen Datensätze gespeichert und im Dateisystem
persistiert. Diese Sätze werden ausgewertet.


Im Dateisystem ist ein Python-Modul ein ganz normaler Ordner. Einzige
Bedingung. Der Ordner muss eine Datei ``__init__.py`` besitzen.

.. code-block:: text

   $ tree abrechnung/
   abrechnung/
   └── __init__.py

   0 directories, 1 file

Im ersten Schritt schreiben wir nur den Author und die Versionsnummer in die
``__init__.py``:

.. code-block:: python

   VERSION='1.0'
   AUTHOR='Christoph Glaubitz'

Das Modul kann nun genau wie eine einzelne Datei verwendet werden.

.. code-block:: python

   >>> import abrechnung
   >>> abrechnung.VERSION
   '1.0'
   >>> abrechnung.AUTHOR
   'Christoph Glaubitz'

Wir strukturieren das Modul so, dass die Datenhalde und die logik zum Auswerten
der Daten in unterschiedliche Dateien aufgeteilt werden.

.. code-block:: text
   
   $ tree abrechnung/
   abrechnung/
   ├── auswertung.py
   ├── daten.py
   └── __init__.py 

Schauen wir uns zuerst die ``daten.py`` an. Die Datei einhält eine Klasse
``Data``, die die eigentlichen Daten hält und sich um deren persistierung
kümmert.

.. code-block:: python
   
   # -*- coding: utf-8 -*-
   import pickle
   from datetime import datetime


   class Data(object):
       """Data object"""

       def __init__(self, fname=None):
           if not fname:
               self._data = {}
               self.fname = None
           else:
               # if fname is set, try to load id
               try:
                   with open(fname, 'rb') as fd:
                       self._data = pickle.load(fd)
               except:
                   # if loading does not work, _data is empty,
                   # but fname is set, for storing purposes.
                   self._data = {}
                   self.fname = fname

       def add(self, user, value):
           """Adding a new accounting value for an single user"""
           assert isinstance(user, str)
           assert isinstance(value, float)
           val = {'value': value,
                  'valuta': datetime.now()}
           if not user in self._data:
               # if user does not exists in _data yet, create an empty list
               self._data[user] = []
           self._data[user].append(val)

       def store(self):
           """Write self._data to file self.fname if set"""
           assert self.fname is not None

           with open(self.fname, 'wb') as fd:
               pickle.dump(self._data, fd)

       def load(self, fname):
           """Try to load _data from file.
           Usefull if fname was not known during creation."""
           self.fname = fname
           try:
               with open(fname, 'rb') as fd:
                   self._data = pickle.load(fd)
           except:
               pass

Im Interpreter kann die Klasse ``Data`` schon verwendet werden:

.. code-block:: python

   >>> from abrechnung.daten import Data
   >>> d = Data('/tmp/my_data.p')
   >>> d._data
   {}
   >>> d.add('chris', 1.0)
   >>> d.add('chris', 2.0)
   >>> d._data
   {'chris': [{'valuta': datetime.datetime(2014, 6, 13, 13, 34, 24, 480439), 'value': 1.0},
              {'valuta': datetime.datetime(2014, 6, 13, 13, 34, 29, 285592), 'value': 2.0}]}
   >>> d.store()

Im internen Dictionary wurden nun zwei Einträge für den User chris angelegt. Anschließend wurden die Daten in die Datei ``/tmp/my_data.p`` persistiert.

.. code-block:: sh

   $ ls -l /tmp/my_data.p
   -rw-r--r-- 1 chris chris 144 Jun 13 13:34 /tmp/my_data.p

In einer neuen Interpreter-Session kann diese Datei nun geladen werden:

.. code-block:: python
   
   >>> from pprint import pprint
   >>> from abrechnung.daten import Data
   >>> d = Data('/tmp/my_data.p')
   >>> pprint(d._data)
   {'chris': [{'value': 1.0,
               'valuta': datetime.datetime(2014, 6, 13, 13, 34, 24, 480439)},
              {'value': 2.0,
               'valuta': datetime.datetime(2014, 6, 13, 13, 34, 29, 285592)}]}

Mittels der Methode ``load`` läßt sich der interne Stand wieder auf den der
Datei zurücksetzen. (weiter in der selben Interpreter-Session)

.. code-block:: python

   >>> d.add('another_user', 3.0)
   >>> pprint(d._data)
   {'another_user': [{'value': 3.0,
                      'valuta': datetime.datetime(2014, 6, 13, 13, 41, 7, 758881)}],
    'chris': [{'value': 1.0,
               'valuta': datetime.datetime(2014, 6, 13, 13, 34, 24, 480439)},
              {'value': 2.0,
               'valuta': datetime.datetime(2014, 6, 13, 13, 34, 29, 285592)}]}
   >>> d.load('/tmp/my_data.p')
   >>> pprint(d._data)
   {'chris': [{'value': 1.0,
               'valuta': datetime.datetime(2014, 6, 13, 13, 34, 24, 480439)},
              {'value': 2.0,
               'valuta': datetime.datetime(2014, 6, 13, 13, 34, 29, 285592)}]}


Was hat es mit dem PYTHONPATH auf sich?
---------------------------------------

TODO
