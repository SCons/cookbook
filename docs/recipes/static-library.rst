Building a Static Library
-------------------------

.. code:: python

   env = Environment()
   env.StaticLibrary(target='foo', source=Split('l1.c l2.c'))
   env.StaticLibrary(target='bar', source=['l3.c', 'l4.c'])

