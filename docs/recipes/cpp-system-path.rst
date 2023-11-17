Search a custom directory for system .h files
---------------------------------------------

The following `tool <https://scons.org/doc/production/HTML/scons-man.html#tools>`_ adds a construction variable ``$CPPSYSPATH`` that works similarly to ``$CPPPATH``:

``site_scons/site_tools/CppSystemPath.py``

.. code-block:: python

    def exists():
        return true

    def generate(env):
        env.Append(CPPPATH=['$CPPSYSPATH'])

        env.Replace(
            SYSINCPREFIX='-isystem' if 'msvc' not in env['TOOLS'] else '/external:I',
            SYSINCSUFFIX='',
        )
        env.Replace(_CPPSYSINCFLAGS='${_concat(SYSINCPREFIX, CPPSYSPATH, SYSINCSUFFIX, __env__, RDirs)}')
        env.Append(CPPFLAGS=['$_CPPSYSINCFLAGS'])

To use it, first add the `tool <https://scons.org/doc/production/HTML/scons-man.html#f-Tool>`_ to the SCons environment:

``SConstruct``

.. code-block:: python

    env = Environment()
    env.Tool('CppSystemPath')
    env.Append(CPPSYSPATH=['custom/system/headers/path'])

Marking header files as system is usually done to silence warnings originating from 3rd party libraries.
By default, Microsoft Visual C++ still shows warnings in those headers.
To fix that, set their warning level to 0:

.. code-block:: python

    if 'msvc' in env['TOOLS']:
        env.Append(CCFLAGS=['/external:W0'])

