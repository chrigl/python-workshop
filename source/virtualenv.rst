.. _`Virtuelles Environment`:

Virtuelles Environment
======================

In quasi jedem Projekt wird man über kurz oder lang externe Pakete verwenden
wollen. In vielen Projekten entscheidet man sich sogar direkt für ein Framework
wie z.B. Pyramid_ des `Pylons Project`_, oder Flask_, um nur wenige zu nennen.
Dieses muss installiert sein, da der eigene Code ohne das gewählte Framework
keinen "Sinn" ergibt.

In den meisten Fällen möchte man in isolierten Umgebungen, die man voll unter
Kontrolle hat, entwickeln. Zum einen möchte man keine Versionskonflikte mit
den von der Distribution gelieferten Paketen haben. Zum anderen sollen Projekte
einfach bootstrappen können. Man denke nur an einen neuen Entwickler oder einer
Neuinstallation des eigenen Systems.

Python2 bietet hier über den Helfer virtualenv Abhilfe. Je nach Distribution
können die Paketnamen unterscheiden. Ubuntu z.B. nennt das Paket
python-virtualenv. Oft wird für python3 ebenfalls ein virtualenv3 zur
Verfügung gestellt. Dies hängt aber von der Distribution ab. Daneben bringt
python3 die Unterstützung für virtuelle Environments unter dem Namen venv_
in der Standard Library mit.

Im Beispiel Ubuntu ist das Standard python-Kommando ein python2, während
das python auf archlinux per default python3 ist.

Erstellen einer isolierten Umgebung
-----------------------------------

Als Beispiel einmal für python2 mit virtualenv und einmal für python3 mittels
venv.

.. code-block:: text

   $ virtualenv my_first_virtualenv
   $ ls -l my_first_virtualenv/
   total 0
   drwxr-xr-x 1 chris chris 224 Jan 31 21:40 bin
   drwxr-xr-x 1 chris chris  18 Jan 31 21:40 include
   drwxr-xr-x 1 chris chris  18 Jan 31 21:40 lib

   $ python3 -m venv my_first_python3_venv
   $ ls -l my_first_python3_venv/
   total 8
   drwxr-xr-x 1 chris chris 174 Jan 31 21:41 bin
   drwxr-xr-x 1 chris chris   0 Jan 31 21:41 include
   drwxr-xr-x 1 chris chris  18 Jan 31 21:41 lib
   lrwxrwxrwx 1 chris chris  41 Jan 31 21:41 lib64 -> /home/chris/tmp/my_first_python3_venv/lib
   -rw-r--r-- 1 chris chris  69 Jan 31 21:41 pyvenv.cfg

In beiden Fällen wird ein neuer Ordner mit einer entsprechenden Struktur
generiert.

Das generieren alleine genügt noch nicht. Damit es benutzt wird, muss es in
jeder Terminalsession aktiviert werden.

Virtuelles Environment aktivieren
---------------------------------

.. code-block:: text

   $ . my_first_virtalenv/bin/active

Der Befehl setzt einige Umgebungsvariable um, so dass alle Pakete und Befehle
nur unterhalb dieses Verzeichnisses angelegt werden. Außerdem wird PATH
entsprechend angepasst.

.. code-block:: text

   $ type -a python
   python is /home/chris/tmp/my_first_virtualenv/bin/python
   python is /usr/bin/python
   
   $ type -a pip
   pip is /home/chris/tmp/my_first_virtualenv/bin/pip

Installiert man nun mittels pip z.B. den interaktiven Python Interpreter
IPython_, wird dieser nur in das gerade ausgewählte Environment installiert.

.. code-block:: text

   $ pip install ipython
   $ find my_first_virtualenv/ -iname "*ipython*"
   my_first_virtualenv/bin/ipython
   my_first_virtualenv/lib/python2.7/site-packages/IPython/...
   ...

Das genannte Verhalten unterscheidet sich nicht zwischen einem python2- und
python3-virtualenv.

Virtuelles Environment deaktivieren/ändern
------------------------------------------

Um ein virtuelles Environment zu wechseln, sollte das alte abgeschaltet und
das neue aktiviert werden. Alternativ zum abschalten kann man natürlich ein
neues Terminalfenster verwenden.

.. code-block:: text

   $ deactivate
   $ . my_first_python3_venv/bin/activate

Zur Kontrolle. Hier gibt es kein ipython.

.. code-block:: text

   $ type -a ipython
   -bash: type: ipython: not found

Oder, Falls das ipython der Distri installiert ist:

.. code-block:: text

   $ type -a ipython
   ipython is /usr/bin/ipython


.. _Pyramid: http://www.pylonsproject.org/projects/pyramid/about
.. _`Pylons Project`: http://www.pylonsproject.org/
.. _Flask: http://flask.pocoo.org/
.. _venv: https://docs.python.org/3/library/venv.html
.. _IPython: http://ipython.org/
