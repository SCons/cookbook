Basic Compilation From Multiple Source Files
--------------------------------------------

.. code:: python

   env = Environment()
   env.Program(target='foo', source=Split('f1.c f2.c f3.c'))

