Using Microsoft Visual C++ precompiled headers
----------------------------------------------

Since ``windows.h`` includes everything and the kitchen sink, it can
take quite some time to compile it over and over again for a bunch of
object files, so Microsoft provides a mechanism to compile a set of
headers once and then include the previously compiled headers in any
object file. This technology is called precompiled headers (PCH). The
general recipe is to create a file named ``StdAfx.cpp`` that includes a
single header named ``StdAfx.h``, and then include every header you want
to precompile in ``StdAfx.h``, and finally include ``"StdAfx.h`` as the
first header in all the source files you are compiling to object files.
For example:

``StdAfx.h``:

.. code:: C++

   #include <windows.h>
   #include <my_big_header.h>

``StdAfx.cpp``:

.. code:: C++

   #include <StdAfx.h>

``Foo.cpp``:

.. code:: C++

   #include <StdAfx.h>

   /* do some stuff */

``Bar.cpp``:

.. code:: C++

   #include <StdAfx.h>

   /* do some other stuff */

``SConstruct``:

.. code:: python

   env=Environment()
   env['PCHSTOP'] = 'StdAfx.h'
   env['PCH'] = env.PCH('StdAfx.cpp')[0]
   env.Program('MyApp', ['Foo.cpp', 'Bar.cpp'])

For more information see the documentation for the ``PCH``
builder, and the ``$PCH`` and ``$PCHSTOP``
construction variables. To learn about the details of precompiled
headers consult the MSDN documentation for ``/Yc``, ``/Yu``, and
``/Yp``.

