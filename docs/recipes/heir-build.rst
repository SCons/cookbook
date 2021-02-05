Creating a Hierarchical Build
-----------------------------

Notice that the file names specified in a subdirectory's SConscript file
are relative to that subdirectory.

``SConstruct``:

.. code:: python

   env = Environment()
   env.Program(target='foo', source='foo.c')

   SConscript('sub/SConscript')

``sub/SConscript``:

.. code:: python

   env = Environment()
   # Builds sub/foo from sub/foo.c
   env.Program(target='foo', source='foo.c')

   SConscript('dir/SConscript')

``sub/dir/SConscript``:

.. code:: python

   env = Environment()
   # Builds sub/dir/foo from sub/dir/foo.c
   env.Program(target='foo', source='foo.c')

