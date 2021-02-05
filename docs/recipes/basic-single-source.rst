Basic Compilation From a Single Source File
-------------------------------------------

.. code:: python

   env = Environment()
   env.Program(target='foo', source='foo.c')

Note: Build the file by specifying the target as an argument
(``scons foo`` or ``scons foo.exe``) or by specifying the
current directory as the target (``scons .``).

