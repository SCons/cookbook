Setting a Compilation Flag for an Environment
---------------------------------------------

.. code:: python

   env = Environment(CCFLAGS='-g')
   env.Program(target='foo', source='foo.c')

