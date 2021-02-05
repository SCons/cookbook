Defining Your Own Builder Object
--------------------------------

Notice that when you invoke the Builder, you can leave off the target
file suffix, and ``scons`` will add it automatically.

.. code:: python

   bld = Builder(
       action='pdftex < $SOURCES > $TARGET',
       suffix='.pdf',
       src_suffix='.tex'
   )
   env = Environment(BUILDERS={'PDFBuilder': bld})
   env.PDFBuilder(target='foo.pdf', source='foo.tex')

   # The following creates "bar.pdf" from "bar.tex"
   env.PDFBuilder(target='bar', source='bar')

Note that the above initialization replaces the default dictionary of
Builders, so this construction environment can not be used call Builders
like ``Program``, ``Object``, ``StaticLibrary`` etc. See the next example for
an alternative.

