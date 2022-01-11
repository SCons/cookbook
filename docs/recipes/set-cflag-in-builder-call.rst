Setting a Compilation Flag while calling a Builder
--------------------------------------------------

.. code:: python

   env = Environment(CCFLAGS='--verbose')
   # Note you can just add any Environment() variables to your builder call and it will
   # overwrite them. There will be no --verbose on the C Compiler command line here
   env.Program(target='foo', source='foo.c', CCFLAGS='-g')
   # You can also append to them in most cases as follows. There
   # will be --verbose on the command line here.
   env.Program(target='foo_2',source='foo2.c', CCFLAGS='$CCFLAGS -g')

