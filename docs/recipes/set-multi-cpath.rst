Search Multiple Directories For .h Files
----------------------------------------

.. code:: python

   env = Environment(CPPPATH=['include1', 'include2'])
   env.Program(target='foo', source='foo.c')

or

.. code:: python

   env = Environment()
   env.AppendENVPath(CPPPATH=['include1', 'include2'])
   env.Program(target='foo', source='foo.c')

