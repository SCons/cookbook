Setting a Compilation Flag in an Override
-----------------------------------------

.. code:: python

   env = Environment()
   env.Program(target='foo', source='foo.c', CCFLAGS='-g')

