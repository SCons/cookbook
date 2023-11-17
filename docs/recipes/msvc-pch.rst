Using Microsoft Visual C++ precompiled headers
----------------------------------------------

Since ``windows.h`` includes everything and the kitchen sink, it can
take quite some time to compile it over and over again for a bunch of
object files, so Microsoft provides a mechanism to compile a set of
headers once and then include the previously compiled headers in any
object file. This technology is called precompiled headers (PCH).
The general recipe is to compile some code which is expected to be
unchanging with a special flag which creates the precompiled header
file, then include that file when building everyting else.
Often, the way this is done is to make a simple C++ source file
which includes a header which includes all the non-changing headers
you want to precompile.
If you create a project in Visual Studio 2019 or later,
it automatically sets up such a header, naming it ``pch.h``,
so the same name will be used here, but the name is not magical
(prior to 2019, it was named ``stdafx.h``).
You can include this special header as the first include in the
source files you want to have use this.
For example:

``pch.h``:

.. code:: C++

   #include <windows.h>
   #include <my_big_header.h>

``pch.cpp``:

.. code:: C++

   #include <pch.h>

``Foo.cpp``:

.. code:: C++

   #include <pch.h>

   /* do some stuff */

``Bar.cpp``:

.. code:: C++

   #include <pch.h>

   /* do some other stuff */

``SConstruct``:

.. code:: python

   env=Environment()
   env['PCHSTOP'] = 'pch.h'
   env['PCH'] = env.PCH('pch.cpp')[0]
   env.Program('MyApp', ['Foo.cpp', 'Bar.cpp'])

For more information see the documentation for the
`PCH <https://scons.org/doc/production/HTML/scons-man.html#b-PCH>`_
builder, and the
`$PCH <https://scons.org/doc/production/HTML/scons-man.html#cv-PCH>`_
and
`$PCHSTOP <https://scons.org/doc/production/HTML/scons-man.html#cv-PCHSTOP>`_
construction variables.
To learn about the details of precompiled headers,
consult the Miscroft documentation for the
``/Yc``, ``/Yu``, and ``/Fp`` options.

