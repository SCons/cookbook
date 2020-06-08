SCons Cookbook
==============

To help you get started using ``SCons``, here are some recipes for
doing various things.

.. contents:: Contents

.. TODO: transition to file-per-example:
.. .. include:: recipes/basic_single_source.rst

Basic Compilation From a Single Source File
-------------------------------------------

.. code:: python

   env = Environment()
   env.Program(target='foo', source='foo.c')

Note: Build the file by specifying the target as an argument
(``scons foo`` or ``scons foo.exe``) or by specifying the
current directory as the target (``scons .``).

Basic Compilation From Multiple Source Files
--------------------------------------------

.. code:: python

   env = Environment()
   env.Program(target='foo', source=Split('f1.c f2.c f3.c'))

Setting a Compilation Flag
--------------------------

.. code:: python

   env = Environment(CCFLAGS='-g')
   env.Program(target='foo', source='foo.c')


Search The Local Directory For .h Files
---------------------------------------

Note: You do *not* need to set CCFLAGS to specify ``-I`` options by
hand. ``scons`` will construct the right ``-I`` options from the
contents of CPPPATH.

.. code:: python

   env = Environment(CPPPATH=['.'])
   env.Program(target='foo', source='foo.c')


Search Multiple Directories For .h Files
----------------------------------------

.. code:: python

   env = Environment(CPPPATH=['include1', 'include2'])
   env.Program(target='foo', source='foo.c')


Building a Static Library
-------------------------

.. code:: python

   env = Environment()
   env.StaticLibrary(target='foo', source=Split('l1.c l2.c'))
   env.StaticLibrary(target='bar', source=['l3.c', 'l4.c'])


Building a Shared Library
-------------------------

.. code:: python

   env = Environment()
   env.SharedLibrary(target='foo', source=['l5.c', 'l6.c'])
   env.SharedLibrary(target='bar', source=Split('l7.c l8.c'))


Linking a Local Library Into a Program
--------------------------------------

.. code:: python

   env = Environment(LIBS='mylib', LIBPATH=['.'])
   env.Library(target='mylib', source=Split('l1.c l2.c'))
   env.Program(target='prog', source=['p1.c', 'p2.c'])


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
   env['BUILDERS]['PDFBuilder'] = bld


Defining Your Own Scanner Object
--------------------------------

The following example shows adding an extremely simple scanner
(``kfile_scan``) that doesn't use a search path at all and simply
returns the file names present on any ``include`` lines in the scanned
file. This would implicitly assume that all included files live in the
top-level directory:

.. code:: python

   import re

   include_re = re.compile(r'^include\s+(\S+)$', re.M)

   def kfile_scan(node, env, path, arg):
       contents = node.get_text_contents()
       includes = include_re.findall(contents)
       return env.File(includes)

   kscan = Scanner(
       name='kfile',
       function=kfile_scan,
       argument=None,
       skeys=['.k'],
   )

   scanners = DefaultEnvironment()['SCANNERS']
   scanners.append(kscan)
   env = Environment(SCANNERS=scanners)

   env.Command('foo', 'foo.k', 'kprocess < $SOURCES > $TARGET')

   bar_in = File('bar.in')
   env.Command('bar', bar_in, 'kprocess $SOURCES > $TARGET')
   bar_in.target_scanner = kscan

It is important to note that you have to return a list of File nodes
from the scan function, simple strings for the file names won't do. As
in the examples shown here, you can use the ``env.File``
function of your current construction environment in order to create
nodes on the fly from a sequence of file names with relative paths.

Here is a similar but more complete example that adds a scanner which
searches a path of directories (specified as the MYPATH construction
variable) for files that actually exist:

.. code:: python

   import re
   import os

   include_re = re.compile(r'^include\s+(\S+)$', re.M)

   def my_scan(node, env, path, arg):
       contents = node.get_text_contents()
       includes = include_re.findall(contents)
       if not includes:
           return []
       results = []
       for inc in includes:
           for dir in path:
               file = str(dir) + os.sep + inc
               if os.path.exists(file):
                   results.append(file)
                   break
       return env.File(results)

   scanner = Scanner(
       name='myscanner',
       function=my_scan,
       argument=None,
       skeys=['.x'],
       path_function=FindPathDirs('MYPATH'),
   )

   scanners = DefaultEnvironment()['SCANNERS']
   scanners.append(scanner)
   env = Environment(SCANNERS=scanners, MYPATH=['incs'])

   env.Command('foo', 'foo.x', 'xprocess < $SOURCES > $TARGET')

The ``FindPathDirs`` function used in the previous
example returns a function (actually a callable Python object) that will
return a list of directories specified in the MYPATH construction
variable. It lets ``scons`` detect the file ``incs/foo.inc``, even if
``foo.x`` contains the line ``include foo.inc`` only. If you need to
customize how the search path is derived, you would provide your own
``path_function`` argument when creating the Scanner object, as follows:

.. code:: python

   # MYPATH is a list of directories to search for files in
   def pf(env, dir, target, source, arg):
       top_dir = Dir('#').abspath
       results = []
       if 'MYPATH' in env:
           for p in env['MYPATH']:
               results.append(top_dir + os.sep + p)
       return results


   scanner = Scanner(
       name='myscanner',
       function=my_scan,
       argument=None,
       skeys=['.x'],
       path_function=pf
   )


Creating a Hierarchical Build
-----------------------------

Notice that the file names specified in a subdirectory's SConscript file
are relative to that subdirectory.

``SConstruct``:

.. code:: python

   env = Environment()
   env.Program(target='foo', source='foo.c')

   SConscript('sub/SConscript')

``sub/SConscript``:

.. code:: python

   env = Environment()
   # Builds sub/foo from sub/foo.c
   env.Program(target='foo', source='foo.c')

   SConscript('dir/SConscript')

``sub/dir/SConscript``:

.. code:: python

   env = Environment()
   # Builds sub/dir/foo from sub/dir/foo.c
   env.Program(target='foo', source='foo.c')


Sharing Variables Between SConscript Files
------------------------------------------

You must explicitly call ``Export`` and
``Import`` for variables that you want to share between
SConscript files.

``SConstruct``:

.. code:: python

   env = Environment()
   env.Program(target='foo', source='foo.c')

   Export("env")
   SConscript('subdirectory/SConscript')

``subdirectory/SConscript``:

.. code:: python

   Import("env")
   env.Program(target='foo', source='foo.c')


Building Multiple Variants From the Same Source
-----------------------------------------------

Use the ``variant_dir`` keyword argument to the
``SConscript`` function to establish one or more
separate variant build directory trees for a given source directory:

``SConstruct``:

.. code:: python

   cppdefines = ['FOO']
   Export("cppdefines")
   SConscript('src/SConscript', variant_dir='foo')

   cppdefines = ['BAR']
   Export("cppdefines")
   SConscript('src/SConscript', variant_dir='bar')

``src/SConscript``:

.. code:: python

   Import("cppdefines")
   env = Environment(CPPDEFINES=cppdefines)
   env.Program(target='src', source='src.c')

Note the use of the ``Export`` method to set the
``cppdefines`` variable to a different value each time we call the
``SConscript`` function.


Hierarchical Build of Two Libraries Linked With a Program
---------------------------------------------------------

``SConstruct``:

.. code:: python

   env = Environment(LIBPATH=['#libA', '#libB'])
   Export('env')
   SConscript('libA/SConscript')
   SConscript('libB/SConscript')
   SConscript('Main/SConscript')

``libA/SConscript``:

.. code:: python

   Import('env')
   env.Library('a', Split('a1.c a2.c a3.c'))

``libB/SConscript``:

.. code:: python

   Import('env')
   env.Library('b', Split('b1.c b2.c b3.c'))

``Main/SConscript``:

.. code:: python

   Import('env')
   e = env.Clone(LIBS=['a', 'b'])
   e.Program('foo', Split('m1.c m2.c m3.c'))

The ``#`` in the LIBPATH directories specify that they're relative to
the top-level directory, so they don't turn into ``Main/libA`` when
they're used in ``Main/SConscript``

Specifying only 'a' and 'b' for the library names allows ``scons`` to
attach the appropriate library prefix and suffix for the current
platform in creating the library filename (for example, ``liba.a`` on
POSIX systems, ``a.lib`` on Windows).


Customizing construction variables from the command line.
---------------------------------------------------------

The following would allow the C compiler to be specified on the command
line or in the file ``custom.py``.

.. code:: python

   vars = Variables('custom.py')
   vars.Add('CC', 'The C compiler.')
   env = Environment(variables=vars)
   Help(vars.GenerateHelpText(env))

The user could specify the C compiler on the command line:

::

   scons "CC=my_cc"

or in the ``custom.py`` file:

.. code:: python

   CC = 'my_cc'

or get documentation on the options:

::

   $ scons -h

   CC: The C compiler.
       default: None
       actual: cc


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


Using Microsoft Visual C++ external debugging information
---------------------------------------------------------

Since including debugging information in programs and shared libraries
can cause their size to increase significantly, Microsoft provides a
mechanism for including the debugging information in an external file
called a PDB file. ``scons`` supports PDB files through the $PDB
construction variable.

``SConstruct``:

.. code:: python

   env=Environment()
   env['PDB'] = 'MyApp.pdb'
   env.Program('MyApp', ['Foo.cpp', 'Bar.cpp'])

