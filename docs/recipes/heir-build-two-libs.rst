Hierarchical Build of Two Libraries Linked With a Program
---------------------------------------------------------

``SConstruct``:

.. code:: python

   env = Environment(LIBPATH=['#libA', '#libB'])
   Export('env')
   SConscript('libA/SConscript')
   SConscript('libB/SConscript')
   SConscript('Main/SConscript')

``libA/SConscript``:

.. code:: python

   Import('env')
   env.Library('a', Split('a1.c a2.c a3.c'))

``libB/SConscript``:

.. code:: python

   Import('env')
   env.Library('b', Split('b1.c b2.c b3.c'))

``Main/SConscript``:

.. code:: python

   Import('env')
   e = env.Clone(LIBS=['a', 'b'])
   e.Program('foo', Split('m1.c m2.c m3.c'))

The ``#`` in the ``LIBPATH`` directories specify that they're relative to
the top-level directory, so they don't turn into ``Main/libA`` when
they're used in ``Main/SConscript``

Specifying only 'a' and 'b' for the library names allows ``scons`` to
attach the appropriate library prefix and suffix for the current
platform in creating the library filename (for example, ``liba.a`` on
POSIX systems, ``a.lib`` on Windows).

