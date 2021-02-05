Sharing Variables Between SConscript Files
------------------------------------------

You must explicitly call ``Export`` and
``Import`` for variables that you want to share between
SConscript files.

``SConstruct``:

.. code:: python

   env = Environment()
   env.Program(target='foo', source='foo.c')

   Export("env")
   SConscript('subdirectory/SConscript')

``subdirectory/SConscript``:

.. code:: python

   Import("env")
   env.Program(target='foo', source='foo.c')

