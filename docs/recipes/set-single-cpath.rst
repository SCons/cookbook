Search The Local Directory For .h Files
---------------------------------------

Note: You do *not* need to set ``CCFLAGS`` to specify ``-I`` options by
hand. ``scons`` will construct the right ``-I`` options from the
contents of ``CPPPATH``.

.. code:: python

   env = Environment(CPPPATH=['.'])
   env.Program(target='foo', source='foo.c')

or

.. code:: python

   env = Environment()
   env.AppendENVPath(CPPPATH=['.'])
   env.Program(target='foo', source='foo.c')

