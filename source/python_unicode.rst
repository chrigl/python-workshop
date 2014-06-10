Python und Unicode
==================

Es gibt die Datentypen str und unicode.

.. code-block:: python

   >>> st = 'Hällo'
   >>> st
   'H\xc3\xa4llo'
   >>> uni = u'Hällo'
   >>> uni
   u'H\xe4llo'
   >>> print(st)
   Hällo
   >>> print(uni)
   Hällo

Gleich???? ... Nöö

.. code-block:: python

   >>> len(st)
   6
   >>> len(uni)
   5

huch...
Unicode ist utf-kodiert

.. code-block:: python

   >>> uni = u'Hello \u262d'
   >>> print(uni)
   Hello ☭
   >>> len(uni)
   7

NIEMALS string und unicode mixen!!!
Einfache funktion die etwas ausgibt

.. code-block:: python

   >>> def printer(s1, s2):
   ...     print('This is printer: %s %s' % (s1, s2))
   ...
   >>> printer('Hello', 'World')
   This is printer: Hello World
   >>> printer('Hello', u'World')
   This is printer: Hello World
   >>> printer('Hällo', 'Wörld')
   This is printer: Hällo Wörld
   >>> printer(u'Hello', u'World')                                                 
   This is printer: Hello World
   >>> printer(u'Hello', 'World')
   This is printer: Hello World

Hä? Wieso, der code läuft doch?
Schon seit Jahren.

Booom

.. code-block:: python

   >>> printer('Hällo', u'Wörld')
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
     File "<stdin>", line 2, in printer
   UnicodeDecodeError: 'ascii' codec can't decode byte 0xc3 in position 18: ordinal not in range(128)

Konvertierung:
unicode zu string

.. code-block:: python

   >>> uni = u'Hello \u262d'
   >>> st = uni.encode('utf-8')
   >>> st
   'Hello \xe2\x98\xad'
   >>> print(st)
   Hello ☭

und zurück

.. code-block:: python

   >>> new_uni = st.decode('utf-8')
   >>> new_uni
   u'Hello \u262d'

Vor dem Konvertieren immer den typ prüfen.

.. code-block:: python

   >>> def converter(in_):
   ...     if isinstance(in_, str):
   ...             return in_.decode('utf-8')
   ...     return in_
   ...
   >>> ret = converter('Hällo')
   >>> ret
   u'H\xe4llo'
   >>> ret = converter(u'Hällo')
   >>> ret
   u'H\xe4llo'

Oder der weg anders herum, wenn man sich entscheidet intern nur mit str
zu arbeiten.

.. code-block:: python

   >>> def converter(in_):
   ...     if isinstance(in_, unicode):
   ...             return in_.encode('utf-8')
   ...     return in_
   ...
   >>> ret = converter('Hällo')
   >>> ret
   'H\xc3\xa4llo'
   >>> ret = converter(u'Hällo')
   >>> ret
   'H\xc3\xa4llo'

Immer noch nicht fertig?

.. code-block:: python

   >>> utf = open('test.utf8', 'r').read()
   >>> lat = open('test.latin1', 'r').read()
   >>> utf
   'H\xc3\xa4llo\n'
   >>> lat
   'H\xe4llo\n'

Hrmpf... converter hilft hier nicht
Man muss wissen, dass latin1 reingekommen ist.

.. code-block:: python

   >>> uni = y.decode('latin1')
   >>> uni
   u'H\xe4llo\n'

Und das ganze jetzt in einen utf-8 konformen string verwandeln,
zumindest falls nötig

.. code-block:: python

   >>> st = z.encode('utf-8')
   >>> st
   'H\xc3\xa4llo\n'

In python3 sind normale strings utf-8 encodiert.
Bringt andere Probleme mit sich. Vor allem auf Unix-Systemen, deren locale
nicht UTF-8 ist. z.B. beim Scriptaufruf durch cron. Oder wann man latin1
codierte Dateien öffnen will.

Das Beispiel von vorhin.

.. code-block:: python

   >>> utf = open('test.utf8', 'r').read()
   >>> utf
   'Hällo\n'
   >>> len(utf)
   6

Alles gut soweit. Aber was passiert wenn eine latin1-Datei gelesen wird?
...
Boom

.. code-block:: python

   >>> lat = open('test.latin1', 'r').read()
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
     File "/usr/lib/python3.4/codecs.py", line 313, in decode
       (result, consumed) = self._buffer_decode(data, self.errors, final)
   UnicodeDecodeError: 'utf-8' codec can't decode byte 0xe4 in position 1: invalid continuation byte

Auch hier taucht das Problem erst wieder genau an der Stelle auf wenn die
"falsch" kodierte Datei > ascii-Zeichen verwendet.
Kann also wieder dazu führen, dass code jahrelang läuft und irgendwann
knallt.

Abhilfe?
Dateien als binary lesen.

.. code-block:: python

   >>> lat = open('test.latin1', 'rb').read()
   >>> lat
   b'H\xe4llo\n'
   >>> un = lat.decode('latin1')
   >>> un
   'Hällo\n'

Modul codecs

.. code-block:: python

   >>> import codecs
   >>> un = codecs.open('test.latin1', 'r', encoding='latin1').read()
   >>> un
   'Hällo\n'
