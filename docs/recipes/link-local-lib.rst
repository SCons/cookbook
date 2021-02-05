Linking a Local Library Into a Program
--------------------------------------

.. code:: python

   env = Environment(LIBS='mylib', LIBPATH=['.'])
   env.Library(target='mylib', source=Split('l1.c l2.c'))
   env.Program(target='prog', source=['p1.c', 'p2.c'])

