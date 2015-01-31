Paketierung von Modulen
=======================

Wird ein Projekt etwas größer, gelangt man schnell an den Punkt an dem
man seinen Code paketieren möchte um diesen vernünftig installieren zu
können. Wie wir schon gesehen haben, können python-Pakete mittels des
Kommandos pip installiert werden.

.. code-block:: python
   
   $ pip install ipython

installiert z.B. das Paket ipython (incl. aller Abhängigkeiten) und generiert
automatisch das executable.

pip sucht per default die angeforderten Pakete im `Python Package Index`_
(aka PyPI oder cheese shop).

.. _`Python Package Index`: http://pypi.python.org/
