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
return a list of directories specified in the ``MYPATH`` construction
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

