Python Skripte schreiben
========================

Bisher haben wir python nur im interaktiven Interpreter wie dem standard python
oder dem etwas vielseitigerem Tool ipython verwendet. Um komplexe Programme zu
schreiben um sie wiederholt ausführen zu können, muss der Code in Scripte
gepackt werden. Python-Dateien Enden in der Regel auf .py z.B. test.py. Diese
Regel ist vor allem auf Module (mehr dazu später) anzuwenden. Einfache
python-Scripte, die z.B. in ~/bin/ landen, erhalten keinen Suffix. Dies hat
keine technischen Gründe, sondern entspricht eher der Unix-Philosophie. So
haben fast alle Dateien unter /usr/bin keinen Suffix, auch wenn es sich dabei
um shell-Scripte handelt.

Shebang
-------

Wie für viele andere Scripte auch, sollte als Shebang

.. code-block:: text

   #!/usr/bin/env INTERPRETER  # hier python

verwendet werden. Dies hat vor allem den Vorteil, dass das Script einfach in
dem standard System-python, als auch in einem virtualenv ausgeführt werden
kann, ohne den Quellcode ändern zu müssen. Ein Beispiel wäre ein virtualenv für
die Entwicklung. Oder das Script läuft nur in python3 während das System-Python
in den aller meisten Distributionen noch python2 ist.

Einheitliche Einrückung
-----------------------

Wie schon angesprochen basiert python auf der Einrückung von Blöcken an statt
Klammern zu verwenden. Am besten hält man sich an die Vorgaben aus pep8.
Wichtig ist es, bei jedem Editieren die selben Einstellungen zu verwenden und
nicht Leerzeichen und Tabs zu mischen. Dafür macht es Sinn, den Editor der Wahl
entsprechend zu konfigurieren.
Als Hilfestellung für andere Entwickler empfielt es sich eine Modeline am Ende
der Datei Einzufügen:

.. code-block:: text

   # vim: set tabstop=4 shiftwidth=4 softtabstop=4 expandtab:

Auch wenn die modeline je nach Konfiguration von vim nicht geladen wird, oder
aber ein ganz anderer Editor zum Einsatz kommt, hilft diese Angabe auch wenn
das Script auf einem anderen als dem Konfigurierten Entwicklungshost zu
editieren.

Beispiel mit Generatoren
------------------------

.. code-block:: python
   :linenos:
   
   def adder(in_):
       for elem in in_:
           yield elem + 1


   def multiplier(in_):
       for elem in in_:
           yield elem * 2


   if __name__ == '__main__':
       orig_list = list(range(1000))

       for elem in multiplier(adder(orig_list)):
           print(elem)

Das Statement ``if __name__ == '__main__'`` wird nur angesprungen, wenn das
Script direkt aufgerufen wird. Somit lassen sich ``adder`` und ``multiplier``
auch mittels ``import`` (mehr dazu später) in anderen Programmen verwenden.
Dies ist keineswegs Pflicht gehört aber zu den best-practices.

Dokumentation
-------------

Um sich selber einen Gefallen zu erweisen, sollten Klassen, Methoden und
Funktionen dokumentiert werden. Natürlich dürfen auch Kommentare an kritischen
Code-Teilen nicht fehlen. Für die Dokumentation von Klassen etc. eigenen sich
Docstrings_ besonders.

.. code-block:: python
   :linenos:

   def adder(in_):
       """Iterates over in_ and returns list of in_[N] + 1"""
       for elem in in_:
          yield elem + 1

Das erleichtert nicht nur beim lesen des Codes, sondern kann auch direkt - ohne
Kenntnis des Codes - im Interpreter verwendet werden.

.. code-block:: text

   >>> def adder(in_):
   ...     """Iterates over in_ and returns list of in_[N] + 1"""
   ...     pass  # just an example
   >>> adder.__doc__
   'Iterates over in_ and returns list of in_[N] + 1'
   >>> help(adder)
   Help on function adder in module __main__:

   adder(in_)
       Iterates over in_ and returns list of in_[N] + 1

.. _Docstrings: http://legacy.python.org/dev/peps/pep-0257/
