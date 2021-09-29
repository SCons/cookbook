Adding Your Own Builder Object to an Environment
------------------------------------------------

.. code:: python

   bld = Builder(
       action='pdftex < $SOURCES > $TARGET'
       suffix='.pdf',
       src_suffix='.tex'
   )
   env = Environment()
   env.Append(BUILDERS={'PDFBuilder': bld})
   env.PDFBuilder(target='foo.pdf', source='foo.tex')
   env.Program(target='bar', source='bar.c')

You also can use other Pythonic techniques to add to the BUILDERS
construction variable, such as:

.. code:: python

   env = Environment()
   env['BUILDERS']['PDFBuilder'] = bld

