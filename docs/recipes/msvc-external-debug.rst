Using Microsoft Visual C++ external debugging information
---------------------------------------------------------

Since including debugging information in programs and shared libraries
can cause their size to increase significantly, Microsoft provides a
mechanism for including the debugging information in an external file
called a PDB file. ``scons`` supports PDB files through the ``$PDB``
construction variable.

``SConstruct``:

.. code:: python

   env=Environment()
   env['PDB'] = 'MyApp.pdb'
   env.Program('MyApp', ['Foo.cpp', 'Bar.cpp'])

