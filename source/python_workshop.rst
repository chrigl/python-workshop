Notizen
=======

Was vorkommen soll, nützliche Links und generelle Gedanken

* Pyhton 2 oder 3

  * Hängt vom Anwendungsfall ab.

    * Gibt es schon Code in python2?
    * Unterstützt die Distri python3?
    * Gibt es alle nötigen Pakete/Erweiterungen in python3?
    * Genügt cpython implementierung oder muss es pypy sein?
    * https://wiki.python.org/moin/Python2orPython3

  * Im Zweifelsfall beide Sprachen unterstützen

    * https://docs.python.org/3/howto/pyporting.html
    * http://pythonhosted.org/six/
    * http://python3porting.com/noconv.html

* Syntax

  * Einrückungsblöcke statt klammern
  * Tabs vs Spaces
  * pep8

    * http://legacy.python.org/dev/peps/pep-0008/

* PEP?

  * Index of Python Enhancement Proposals
  * Was soll neu hinzukommen?
  * Was soll wie geändert werden?
  * Was soll entfernt werden?

* Python Package Index

  * Öffentliches Paket Repository wie cpan bei Perl
  * http://pypi.python.org/

* Entwicklungsumgebung

  * vim oder IDE?

    * IDE

      * Eclipse mit PyDev
      * PyCharm
      * Netbeans

    * vim

      * Modul-Verwaltung mit vundle https://github.com/gmarik/Vundle.vim
      * python-mode https://github.com/klen/python-mode

  * Helferlein

    * Sauberes Environment

      * virtualenv (2+3)
      * pyvenv (nur 3)
      * . myenv/bin/activate

    * Pakete Manager

      * easy_install (selten benötigt, obsolete)
      * pip

    * Interaktiver Interpreter

      * python
      * ipython (pip install ipython)

    * Debugger

      * pdb
      * ipdb (pip install ipdb)

    * Code hilfen

      * pylint
      * pep8
      * rope (für Refactoring)
      * Kommen z.B. alle mit dem vim python-mode

* Module

  * my_module/__init__.py

* Test Driven Development?

  * py.test https://pypi.python.org/pypi/pytest
  * nose https://pypi.python.org/pypi/nose
  * ... und viele mehr

* Generatoren

  * http://www.dabeaz.com/generators/

# vim: set tabstop=2 softtabstop=2 shiftwidth=2 expandtab:
