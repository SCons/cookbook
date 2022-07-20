Building Multiple Variants From the Same Source
-----------------------------------------------

Use the ``variant_dir`` keyword argument to the
`SConscript <https://scons.org/doc/production/HTML/scons-man.html#f-SConscript>`_
function to establish one or more
separate variant build directory trees for a given source directory:

``SConstruct``

.. code-block:: python

   cppdefines = ['FOO']
   SConscript('src/SConscript', variant_dir='foo', exports='cppdefines')

   cppdefines = ['BAR']
   SConscript('src/SConscript', variant_dir='bar', exports='cppdefines')

``src/SConscript``

.. code-block:: python

   Import('cppdefines')
   env = Environment(CPPDEFINES=cppdefines)
   env.Program(target='src', source='src.c')

Note the use of the ``exports`` keyword argument to pass a
different value for the ``cppdefines`` variable value each time we call the
``SConscript`` function.

