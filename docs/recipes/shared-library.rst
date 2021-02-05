Building a Shared Library
-------------------------

.. code:: python

   env = Environment()
   env.SharedLibrary(target='foo', source=['l5.c', 'l6.c'])
   env.SharedLibrary(target='bar', source=Split('l7.c l8.c'))

