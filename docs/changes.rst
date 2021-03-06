.. _atelier.changes: 

=======================
Changes in `atelier`
=======================

Version 0.0.20 (not released)
=============================

Version 0.0.19 (released 2016-03-08)
====================================

- New functions :func:`atelier.utils.dict_py2`,
  :func:`atelier.utils.list_py2` and :func:`atelier.utils.tuple_py2` are
  required for Lino's test suite.

Version 0.0.18 (released 2016-03-04)
====================================

- New function :func:`atelier.utils.last_day_of_month`.


Version 0.0.17 (released 2016-02-15)
====================================

- Subtle change in :attr:`docs_rsync_dest
  <atelier.fablib.env.docs_rsync_dest>`: until now it was not possible
  to specify a template without any placeholder (as the one in the
  example on https://github.com/lsaffre/dblog)

- Started to replace fabric by invoke. This is not finished. For the
  moment you should continue to use the ``fab`` commands. But soon
  they will be replaced by ``inv`` commands.


Version 0.0.16 (released 2015-12-04)
====================================

- :mod:`atelier.fablib` no longer tries to import
  `django.utils.importlib`. (Dropped support for Python 2.6)

- Fixed :ticket:`553`. The :cmd:`fab bd` command failed to call
  :meth:`load_fabfile <atelier.projects.Project.load_fabfile>` when
  trying to write the `README.rst` file. This didn't disturb anybody
  until now because I have a :xfile:`~/.atelier/config.py` file (and
  when you have such a file, all projects are automatically loaded,
  including :meth:`load_fabfile
  <atelier.projects.Project.load_fabfile>`.

- Fixed :ticket:`533`. :cmd:`fab bd` failed when the repository was in
  a directory using a symbolic link because Python got hassled when
  importing the main module. :mod:`atelier.projects` now resolves the
  `project_dir`.


Version 0.0.15 (released 2015-06-10)
====================================

New setting :attr:`atelier.fablib.env.locale_dir`. Until now
:command:`fab mm` always wrote the locale files into a subdirectory of
the main module. Now a project can specify an arbitrary location. This
was necessary for Django 1.7 where you cannot have plugins named
`foo.modlib.bar` if you also have a plugin whose full name is `foo`
(:blogref:`20150427`)

New function `atelier.rstgen.attrtable`.

Version 0.0.14 (released 2015-03-15)
====================================

Importing :mod:`atelier` now automatically adds a codecs writer to
`sys.stdout`.  As a consequence, :mod:`atelier.doctest_utf8` is no
longer needed.


Version 0.0.13 (released 2015-02-14)
====================================

Fixed a bug in :meth:`atelier.test.TestCase.run_subprocess` which
could cause a subprocess to deadlock when it generated more output
than the OS pipe buffer would swallow.

:class:`JarBuilder <atelier.jarbuilder.JarBuilder>` is now in a
separate module, the usage API is slightly changed. Signing with a
timestamp is now optional, and the URL of the TSA can be configured.


Version 0.0.12 (released 2015-02-02)
====================================

Getting Lino to build on Travis CI.  Once again I changed the whole
system of declaring demo projects. The parameter to
:func:`atelier.fablib.add_demo_project` must be a Django settings
module, it cannot be a path.  And
:func:`atelier.fablib.run_in_demo_projects` must set the current
working directory to the :attr:`cache_dir
<lino.core.site.Site.cache_dir>`, not the :attr:`project_dir
<lino.core.site.Site.project_dir>`.


Version 0.0.11 (released :blogref:`20150129`)
==============================================

- Users of :mod:`atelier.fablib` who used "demo databases" (which we
  now call "Django demo projects", see
  :attr:`atelier.fablib.env.demo_projects`) must adapt their
  :xfile:`fabfile.py` as described in :blogref:`20150129`.

- New configuration setting :attr:`atelier.fablib.env.editor_command`.

Version 0.0.10 (released :blogref:`20141229`)
==============================================

Fixes a problem for generating the calendar view of a
:rst:dir:`blogger_year`: the cell for December 29, 2014 was not
clickable even when a blog entry existed.

Version 0.0.9  (released :blogref:`20141226`)
=============================================

- :cmd:`fab blog` failed when the user had only :envvar:`VISUAL` but
  not :envvar:`EDITOR` set (:blogref:`20141227`).

- :cmd:`fab blog` failed when the directory for the current year
  didn't yet exist.  Now it automatically wishes "Happy New Year",
  creates both the directory and the default :file:`index.rst` file
  for that year.

- Removed :srcref:`scripts/shotwell2blog.py` which has now `its own
  repository <https://github.com/lsaffre/shotwell2blog>`_.

- :srcref:`scripts/per_project` no longer stumbles over projects whose
  `revision_control_system` is None.

Version 0.0.8  (released :blogref:`20141226`)
=============================================

- :ref:`fab_commands` can now be invoked from a subdirectory of the
  project's root. And :mod:`atelier.projects` now supports to work in
  undeclared projects even if there is a :xfile:`config.py` file.
  (:blogref:`20141226`)

- New method :meth:`shell_block
  <atelier.sphinxconf.insert_input.Py2rstDirective.shell_block>`.
- `fab docs` renamed to :cmd:`fab bd`, `fab pub` renamed to :cmd:`fab pd`



Version 0.0.7 (released :blogref:`20141222`)
============================================

This is a bugfix release for 0.0.6 which fixes one bug::

  [localhost] local: git tag -a 0.0.6 -m Release atelier 0.0.6.
  fatal: too many params


Version 0.0.6 (released :blogref:`20141222`)
============================================

- The :cmd:`fab release` now also does `git tag`.
- The :cmd:`fab release` command now reminds me of the things to check
  before a release, communicates with PyPI and displays information
  about the last official release.
- Improved the documentation.


Version 0.0.5 (released 20141207)
=================================

Version 0.0.3
==============================

- Fixed `AttributeError: work_root` occuring when there was 
  no `work_root` in user's :xfile:`.fabricrc` file.  
  The `work_root` env setting is no longer used.

- (:blogref:`20140117`) atelier now supports namespace packages
  (and thus the :cmd:`fab summary` fablib command no longer prints "old" and
  "new" version because that would require the Distribution object
  (returned from `pkg_resources.get_distribution`) which afaics makes
  problems for namespace packages.

-   (:blogref:`20130623`) 
    :meth:`atelier.test.TestCase.run_simple_doctests` 
    didn't yet support non-ascii characters.

    Now it does. 
    Had to add a new module :mod:`atelier.doctest_utf8`
    for this. 
    Because we need to run each doctest in a separate subprocess 
    and because the command-line interface
    of `python -m doctest`  has no way to specify an encoding 
    of the input file.


- :func:`atelier.sphinxconf.configure` now 
  automatically adds the intersphinx entries 
  for projects managed in this atelier.


- The `PROJECTS` variable in `/etc/atelier/config.py` is now a list of 
  importable Python module names, and their local path will be 
  automatically extracted. 
  No longer necessary to define a `PROJECTS_HOME`

- `per_project` no longer inserts "fab" as first command.

- Renamed `atelier.test.SubProcessTestCase` to `atelier.test.TestCase`.
  Moved Django-specific methods away to a new module 
  :mod:`djangosite.utils.pythontest`.

Version 0.0.2 (released :blogref:`20130505`)
============================================

- `atelier.test.SubProcessTestCase.run_docs_doctests`
  now activates the Site's default language for each testcase
  (when :mod:`north` is available)

Version 0.0.1 (released :blogref:`20130422`)
============================================

- This project was split out of 
  `djangosite <https://pypi.python.org/pypi/djangosite>`_ in 
  April 2013.
  See :blogref:`20130410`.
  

