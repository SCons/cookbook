Setting a Compilation Flag in an Overrids
-----------------------------------------

.. code:: python

   env = Environment()
   env.Program(target='foo', source='foo.c', CCFLAGS='-g')

