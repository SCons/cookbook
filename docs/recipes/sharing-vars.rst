Sharing Variables Between SConscript Files
------------------------------------------

You must explicitly call
`Export <https://scons.org/doc/production/HTML/scons-man.html#f-Export>`_
and
`Import <https://scons.org/doc/production/HTML/scons-man.html#f-Import>`_
for variables that you want to share between
SConscript files.

``SConstruct``

.. code:: python

   env = Environment()
   env.Program(target='foo', source='foo.c')

   Export('env')
   SConscript('subdirectory/SConscript')

``subdirectory/SConscript``

.. code:: python

   Import('env')
   env.Program(target='foo', source='foo.c')

Alternatively, you can use the ``exports``
keyword argument to pass a variable to an SConscript.
In this case the global definition that would be
affected by calling ``Export`` is not modified.

``SConstruct``

.. code:: python

   env = Environment()
   env.Program(target='foo', source='foo.c')

   SConscript('subdirectory/SConscript', exports='env')
