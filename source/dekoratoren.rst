Dekoratoren
===========

Was ist ein decorator?

Eine gewöhnliche Funktion, die ein callable (funktion) zurück gibt.

Mehr dazu: `Python Decorators in 12 steps`_

In python ist alles ein Objekt, auch eine Funktion!

.. code-block:: python

   >>> def testing(a,b):
   ...     return a+b
   ...
   >>> testing.__class__
   function

Funktionen können also beliebigen Namen (Variablen) zugewiesen werden.

.. code-block:: python

   >>> mytest = testing

Hier ist testing _nicht_ ausgeführt worden, sondern nur intern ein
zusätzlicher Zeiger darauf gesetzt.

.. code-block:: python

   >>> id(testing)
   47683296

   >>> id(mytest)
   47683296

Außerdem kann eine Funktion immer durch eine andere ersetzt werden.
Am Beispiel einer Methode:

.. code-block:: python

   >>> class A(object):
   ...     def test(self, a):
   ...         return a
   ...

   >>> a = A()

   >>> a.test(1)
   1

   >>> def replace_test(self, a):
   ...     return a+1
   ...

Die Methode der Klasse wird ersetzt

.. code-block:: python

   >>> A.test = replace_test

   >>> a2 = A()

   >>> a2.test(1)
   2

Das fertige und bereits benutzte Objekt a bleibt unverändert und gibt
nach wie vor 1 zurück

.. code-block:: python

   >>> a.test(1)
   1

Ziel eines Decorator ist es die Funktion zu ersetzen, die ursprüngliche
Funktionalität aber beizubehalten.
Warum sollte man so etwas tun?
Z.B. soll vor jedem Lauf der eigentlichen Funktion, den Ein- und
Austritt zu protokollieren.

.. code-block:: python

   >>> import time

   >>> def testing(a, b, c=1):
   ...     time.sleep(c)
   ...     return a+b
   ...

Benötigt wird eine Funktion, die eine Funktion entgegen nimmt und eine
Funktion zurück gibt.

.. code-block:: python

   >>> def logit(fn):
   ...     def _logit(*args, **kw):
   ...         print("Enter")
   ...         ret = fn(*args, **kw)
   ...         print("Leave")
   ...         return ret
   ...     return _logit
   ...

Die originale Funktion testing ausgeführt.

.. code-block:: python

   >>> testing(1,2)
   3

testing ersetzt durch wrap von testing durch logit
Da logit nur die geschachtelte (closure) Funktion _logit zurück gibt,
werden weder die prints noch fn (:= testing) aufgeruften.

.. code-block:: python

   >>> testing = logit(testing)

testing kann wie gewohnt aufgerufen werden. Es muss kein weiterer Code
angepasst werden

.. code-block:: python

   >>> testing(1,2)
   Enter
   Leave
   3

In echt ist testing jetzt _logit

.. code-block:: python

   >>> testing.__name__
   '_logit'

Es gibt die @-Notation um Dekoratoren beim definieren von Funktionen und
Methoden einfacher zu verwenden:

.. code-block:: python

   >>> @logit
   ... def new_func(a, b):
   ...     time.sleep(1)
   ...     return a*b
   ...

   >>> new_func(1,2)
   Enter
   Leave
   2

Dies funktioniert genau so für Methoden:

.. code-block:: python

   >>> class A(object):
   ...     @logit
   ...     def testing(self, x):
   ...         print('Running testing with x=%s' % x)
   ...

   >>> a = A()

   >>> a.testing(1)
   Enter
   Running testing with x=1
   Leave

Kann auch als Klasse implementiert werden.

.. code-block:: python

   >>> class logit(object):
   ...     def __init__(self, fn):
   ...         self.fn = fn
   ...     def __call__(self, *args, **kw):
   ...         print("class impl Enter")
   ...         ret = self.fn(*args, **kw)
   ...         print("class impl Leave")
   ...         return ret
   ...

   >>> @logit
   ... def another_func(x):
   ...     print("Running with x=%s" % x)
   ...     return x
   ...

   >>> another_func(1)
   class impl Enter
   Running with x=1
   class impl Leave
   1

Mehrere Dekoratoren für eine Funktion sind möglich.

.. code-block:: python

   >>> def timeit(fn):
   ...     def _timeit(*args, **kw):
   ...         start = time.time()
   ...         ret = fn(*args, **kw)
   ...         end = time.time()
   ...         print("Runtime: %s" % (end-start))
   ...         return ret
   ...     return _timeit
   ...

   >>> def yet_another_func(x):
   ...     time.sleep(1)
   ...     return x*2
   ...

   >>> @logit
   ... @timeit
   ... def yet_another_func(x):
   ...     time.sleep(1)
   ...     return x*2
   ...

   >>> yet_another_func(1)
   class impl Enter
   Runtime: 1.00109910965
   class impl Leave
   2

Weitere Beispiele für Dekoratoren PythonDecoratorLibrary_, `Introduction to Python Decorators`_
Werden in vielen Frameworks für diverse Dinge benutzt.
z.B. `Registrierung von Views in pyramid`_

.. code-block:: python

   >>> from pyramid.view import view_config

   >>> @view_config(renderer='templates/foo.pt')
   >>> def my_view(request):
   ...     return {'foo':1, 'bar':2}

Es wird eine View my_view registriert, die das Template foo.pt benutzt.
Vom Entwickler muss nur ein Dictionary zurückgegeben werden, dessen Werte
im Template verwendet werden. Um den Rest kümmert sich das Framework.

Nachteil von Dekoratoren: Bei Unittests müssen alle Dekoratoren mit
getestet werden.
Im pyramid-Beispiel könnte man mit einem naiven Dekorator nicht einfach
testen ob my_view(some_request) {'foo':1, 'bar':2} zurück gibt, sondern
ob der gewünschte Response raus kommt.
Der Author von pyramid Chris McDonough hat zu diesem Zweck venusian_
entwickelt.

.. _PythonDecoratorLibrary: https://wiki.python.org/moin/PythonDecoratorLibrary
.. _`Python Decorators in 12 steps`: http://simeonfranklin.com/blog/2012/jul/1/python-decorators-in-12-steps
.. _`Introduction to Python Decorators`: http://www.artima.com/weblogs/viewpost.jsp?thread=240808
.. _`Registrierung von Views in pyramid`: https://github.com/Pylons/pyramid/blob/master/pyramid/view.py#L119
.. _venusian: https://github.com/Pylons/venusian
