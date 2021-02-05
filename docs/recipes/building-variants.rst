Building Multiple Variants From the Same Source
-----------------------------------------------

Use the ``variant_dir`` keyword argument to the
``SConscript`` function to establish one or more
separate variant build directory trees for a given source directory:

``SConstruct``:

.. code:: python

   cppdefines = ['FOO']
   Export("cppdefines")
   SConscript('src/SConscript', variant_dir='foo')

   cppdefines = ['BAR']
   Export("cppdefines")
   SConscript('src/SConscript', variant_dir='bar')

``src/SConscript``:

.. code:: python

   Import("cppdefines")
   env = Environment(CPPDEFINES=cppdefines)
   env.Program(target='src', source='src.c')

Note the use of the ``Export`` method to set the
``cppdefines`` variable to a different value each time we call the
``SConscript`` function.

