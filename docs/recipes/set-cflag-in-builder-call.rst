Setting a Compilation Flag while calling a Builder
--------------------------------------------------

You can add any variables to your builder call and it will
override them - only for for that call.
There will be no ``--verbose`` on the C Compiler command line here:

.. code-block:: python

   env = Environment(CCFLAGS='--verbose')
   env.Program(target='foo', source='foo.c', CCFLAGS='-g')


You can also use variable substitution to append to values,
incuding in an override.
There *will* be a ``--verbose`` on the command line here:

.. code-block:: python

   env = Environment(CCFLAGS='--verbose')
   env.Program(target='foo_2',source='foo2.c', CCFLAGS='$CCFLAGS -g')

