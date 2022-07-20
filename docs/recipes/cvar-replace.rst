Replacing Contents of a Construction Variable
---------------------------------------------

The
`Replace <https://scons.org/doc/production/HTML/scons-man.html#f-Replace>`_
method of a construction enviromnent replaces the entire existing value.
It can be convenient to use a Python dictionary and unpacking to set many
values at once:

.. code-block:: python

   values = {"AFLAGS": "ONE", "BFLAGS": "TWO", "EXTRAFLAGS": ["THREE", "FOUR"]}
   env.Replace(**values)

The method replaces the entire value for each variable,
which is the same thing that happens when you use an
override assignment as in this example:

.. code-block:: python

   env = Environment(AFLAGS="ONE", BFLAGS="TWO")
   ...
   env.Program(target='foo', source='foo.c', BFLAGS="THREE")

Sometimes, a construction variable is a container type
and you need to replace only some element of it -
a common case is preprocessor macros which take values (`CPPDEFINES`).
Neither ``Replace`` nor assignment will directly help with that,
nor will using
`AppendUnique <https://scons.org/doc/production/HTML/scons-man.html#f-AppendUnique>`_
since it deals in exact matches - that is, you can
move an element to the end, but not give it a different value.

In this case, it may be useful to use a loop or comprehension
to build a new list and use ``Replace`` or assignment to
set the new list as the value:

.. code-block:: python

   env = Environment(CPPDEFINES=["BUILD", "DEBUG=0", "LINUX=1"])
   ...
   clone = env.Clone()
   cflags = ["DEBUG=1" if "DEBUG" in val else val for val in clone['CPPDEFINES']]
   clone.Replace(CPPDEFINES=cflags)

