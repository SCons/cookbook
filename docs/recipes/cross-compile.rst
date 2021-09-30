Setting up a Cross Compile Environment
--------------------------------------

SCons doesn't have any specific support for cross-compiling,
but it's not too hard to set up. There are a few things
that you need to consider:

1. Cross-compilation tools often have alternate names, for example
   the C++ compiler might be called ``arm-none-eabi-g++`` instead of ``g++``.
   Since SCons won't auto-detect these alternate names, and doesn't
   have a pre-defined toolname-prefix construction variable, you have
   to handle pointing to the right tools yourself by setting the
   appropriate construction variables.

2. Cross-compilation tools often live in a filesystem path of their
   own, not in "standard" locations from the viewpoint of, say a
   Linux system (Windows systems nearly always put an installed
   package in a unique path so this is a bit less of a surprise there).
   For eaxmple, a system might put all the tools in
   ``/opt/arm-none-eabi/bin``.  Even if such a path is in your own
   search path, SCons doesn't use that and instead uses its own
   idea of "standard locations". So you have to add any special path
   to the tools to SCons' idea of locations.

3. Cross-compiled code normally can't be run directly on the host
   system.  It may be able to run in an emulator, or it may need to
   be sent over to the target machine. The upshot of this is that
   any build instructions that depend executing a locally built
   binary need some attention if a cross-compiled build is attempted.
   This could affect things like code- or data- generation tools that
   are made as part of the build, then are called to generate something
   which is used later in the build. It also affects builds which want
   to build test binaries and then execute the tests, all as part
   of the build process.  Both of these scenarios would have to be
   adapted somehow.

Here is a very basic setup example:

.. code:: python

   import os

   env_options = {
      "CC"    : "nios2-linux-gnu-gcc",
      "CXX"   : "nios2-linux-gnu-g++",
      "LD"    : "nios2-linux-gnu-g++",
      "AR"    : "nios2-linux-gnu-ar",
      "STRIP" : "nios2-linux-gnu-strip",
   }

   env = Environment(**env_options)
   env.AppendENVPath('PATH': '/path/to/tools')
