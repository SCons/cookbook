Setting Up a Python Virtualenv for SCons
----------------------------------------

It is often useful to set up a virtualenv when working with a project
that uses SCons to build.  A virtualenv is a way to create an
isolated execution environment - you can install whatever you need
there, and change versions as needed, without affecting anything
else on your system that uses Python.  Here is an example session
for setting one up on a Linux host:

.. code:: shell

   $ cd Work
   $ python -m venv myvenv
   $ source bin/activate
   (myvenv) $ pip list --outdated
   Package    Version Latest Type
   ---------- ------- ------ -----
   pip        19.3.1  21.0.1 wheel
   setuptools 41.6.0  53.0.0 wheel
   WARNING: You are using pip version 19.3.1; however, version 21.0.1 is available.
   You should consider upgrading via the 'pip install --upgrade pip' command.
   (myvenv) $ pip install --upgrade pip setuptools scons
   Collecting pip
     Downloading https://files.pythonhosted.org/packages/fe/ef/60d7ba03b5c442309ef42e7d69959f73aacccd0d86008362a681c4698e83/pip-21.0.1-py3-none-any.whl (1.5MB)
        |................................| 1.5MB 909kB/s
   Collecting setuptools
     Downloading https://files.pythonhosted.org/packages/15/0e/255e3d57965f318973e417d5b7034223f1223de500d91b945ddfaef42a37/setuptools-53.0.0-py3-none-any.whl (784kB)
        |................................| 788kB 1.2MB/s
   Collecting scons
     Using cached https://files.pythonhosted.org/packages/de/8c/4da2c7dd43466383b7ade4189d995c2102248f507af7ba6f456df0854920/SCons-4.1.0.post1-py3-none-any.whl
   Installing collected packages: pip, setuptools, scons
     Found existing installation: pip 19.3.1
       Uninstalling pip-19.3.1:
         Successfully uninstalled pip-19.3.1
     Found existing installation: setuptools 41.6.0
       Uninstalling setuptools-41.6.0:
         Successfully uninstalled setuptools-41.6.0
   Successfully installed pip-21.0.1 scons-4.1.0.post1 setuptools-53.0.0
   (myvenv) $ scons --version
   SCons by Steven Knight et al.:
           SCons: v4.1.0.post1.dc58c175da659d6c0bb3e049ba56fb42e77546cd, 2021-01-20 04:32:28, by bdbaddog on ProDog2020
           SCons path: ['/home/user/Work/myvenv/lib64/python3.8/site-packages/SCons']
   Copyright (c) 2001 - 2021 The SCons Foundation
   (myvenv) $ deactivate
   $

To use this virtualenv for work, repeat the
step to activate it, go to your project, and install
any additional requirements your project may have
(skip this step if there are none).

.. code:: shell

   $ cd Work/myproject
   $ source ~/Work/myvenv/bin/activate
   (myvenv) $ pip install -r requirements.txt


Here is the same session from a Windows host:

.. code:: cmd

   C:\Users\user>cd Work

   C:\Users\user\Work>py -m venv myvenv

   C:\Users\user\Work>.\myvenv\Scripts\activate

   (myvenv) C:\Users\user\Work>pip list --outdated
   Package    Version Latest Type
   ---------- ------- ------ -----
   pip        20.2.3  21.0.1 wheel
   setuptools 49.2.1  53.0.0 wheel
   WARNING: You are using pip version 20.2.3; however, version 21.0.1 is available.
   You should consider upgrading via the 'C:\Users\user\Work\myvenv\Scripts\python.exe -m pip install --upgrade pip' command.

   (myvenv) C:\Users\user\Work\myvenv\Scripts\python.exe -m pip install --upgrade pip setuptools scons
   Collecting pip
     Using cached pip-21.0.1-py3-none-any.whl (1.5 MB)
   Collecting setuptools
     Using cached setuptools-53.0.0-py3-none-any.whl (784 kB)
   Collecting scons
     Using cached SCons-4.1.0.post1-py3-none-any.whl (4.1 MB)
   Installing collected packages: pip, setuptools, scons
     Attempting uninstall: pip
       Found existing installation: pip 20.2.3
       Uninstalling pip-20.2.3:
         Successfully uninstalled pip-20.2.3
     Attempting uninstall: setuptools
       Found existing installation: setuptools 49.2.1
       Uninstalling setuptools-49.2.1:
         Successfully uninstalled setuptools-49.2.1
   Successfully installed pip-21.0.1 scons-4.1.0.post1 setuptools-53.0.0

   (myvenv) C:\Users\user\Work>scons --version
   SCons by Steven Knight et al.:
           SCons: v4.1.0.post1.dc58c175da659d6c0bb3e049ba56fb42e77546cd, 2021-01-20 04:32:28, by bdbaddog on ProDog2020
           SCons path: ['c:\\users\\user\\work\\myvenv\\lib\\site-packages\\SCons']
   Copyright (c) 2001 - 2021 The SCons Foundation

   (myvenv) C:\Users\user\Work>
