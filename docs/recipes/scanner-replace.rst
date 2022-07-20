Selecting an Alternate Scanner
------------------------------

Sometimes you want to replace the existing scanner for
certain types of files with an alternative.
In the case where the scanner's `function` attribute
is a dictionary, that dictionary serves as a dispatcher
to other scanner functions, and you can simply update
the dictionary.  This example depends on the use of the
global definition of ``SourceFileScanner``, which exposes
an `add_scanner` method, normally intended to add
*additional* scanner mappings, but just as usable
to do replacements.

In this case, the alternative scanner for C/C++ type files
is patched in in place of the default:

.. code-block:: python

   import SCons.Scanner.C
   CConditionalScanner = SCons.Scanner.C.CConditionalScanner()
   SourceFileScanner.add_scanner('.c', CConditionalScanner)
   # do more of those as needed for other suffixes (perhaps see $CPPSUFFIXES)
