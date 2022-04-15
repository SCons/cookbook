Setting Up a Python Virtualenv for SCons
----------------------------------------

It is often useful to set up a virtualenv when working with a project
that uses SCons to build.  A virtualenv is a way to create an
isolated execution environment - you can install whatever you need
there, and change versions as needed, without affecting anything
else on your system that uses Python.  Here is an example session:

.. tabs::
   .. code-tab:: console Linux

      $ cd Work
      $ python -m venv myvenv
      $ source myvenv/bin/activate
      (myvenv) $ pip list --outdated
      Package    Version Latest Type
      ---------- ------- ------ -----
      pip        19.3.1  22.0.4 wheel
      setuptools 41.6.0  62.1.0 wheel
      WARNING: You are using pip version 19.3.1; however, version 22.0.4 is available.
      You should consider upgrading via the 'pip install --upgrade pip' command.
      (myvenv) $ pip install --upgrade pip setuptools scons
      Collecting pip
        Downloading pip-22.0.4-py3-none-any.whl (2.1 MB)
      Collecting setuptools
        Downloading setuptools-62.1.0-py3-none-any.whl (1.1 MB)
      Collecting scons
        Downloading SCons-4.3.0-py3-none-any.whl (4.2 MB)
      Installing collected packages: pip, setuptools, scons
        Found existing installation: pip 19.3.1
          Uninstalling pip-19.3.1:
            Successfully uninstalled pip-19.3.1
        Found existing installation: setuptools 41.6.0
          Uninstalling setuptools-41.6.0:
            Successfully uninstalled setuptools-41.6.0
      Successfully installed pip-22.0.4 scons-4.3.0 setuptools-62.1.0
      (myvenv) $ scons --version
      SCons by Steven Knight et al.:
              SCons: v4.3.0.559790274f66fa55251f5754de34820a29c7327a, Tue, 16 Nov 2021 19:09:21 +0000, by bdeegan on octodog
              SCons path: ['/home/user/Work/myvenv/lib64/python3.8/site-packages/SCons']
      Copyright (c) 2001 - 2021 The SCons Foundation
      (myvenv) $ deactivate
      $

   .. code-tab:: doscon Windows

      C:\Users\user>cd Work
      C:\Users\user\Work>py -m venv myvenv
      C:\Users\user\Work>.\myvenv\Scripts\activate
      (myvenv) C:\Users\user\Work>pip list --outdated
      Package    Version Latest Type
      ---------- ------- ------ -----
      pip        20.2.3  22.0.4 wheel
      setuptools 49.2.1  62.1.0 wheel
      WARNING: You are using pip version 20.2.3; however, version 22.0.4 is available.
      You should consider upgrading via the 'C:\Users\user\Work\myvenv\Scripts\python.exe -m pip install --upgrade pip' command.
      (myvenv) C:\Users\user\Work\myvenv\Scripts\python.exe -m pip install --upgrade pip setuptools scons
      Collecting pip
        Downloading pip-22.0.4-py3-none-any.whl (2.1 MB)
      Collecting setuptools
        Downloading setuptools-62.1.0-py3-none-any.whl (1.1 MB)
      Collecting scons
        Downloading SCons-4.3.0-py3-none-any.whl (4.2 MB)
      Installing collected packages: pip, setuptools, scons
        Attempting uninstall: pip
          Found existing installation: pip 20.2.3
          Uninstalling pip-20.2.3:
            Successfully uninstalled pip-20.2.3
        Attempting uninstall: setuptools
          Found existing installation: setuptools 49.2.1
          Uninstalling setuptools-49.2.1:
            Successfully uninstalled setuptools-49.2.1
      Successfully installed pip-22.0.4 scons-4.3.0 setuptools-62.1.0
      (myvenv) C:\Users\user\Work>scons --version
      SCons by Steven Knight et al.:
              SCons: v4.3.0.559790274f66fa55251f5754de34820a29c7327a, Tue, 16 Nov 2021 19:09:21 +0000, by bdeegan on octodog
              SCons path: ['c:\\users\\user\\work\\myvenv\\lib\\site-packages\\SCons']
      Copyright (c) 2001 - 2021 The SCons Foundation
      (myvenv) C:\Users\user\Work> deactivate
      C:\Users\user\Work>

   .. code-tab:: console MacOS

      user@host ~ % cd Work
      user@host Work % python3 -m venv myvenv
      user@host Work % source myvenv/bin/activate
      (myvenv) user@host Work % pip list --outdated
      Package    Version Latest Type
      ---------- ------- ------ -----
      pip        21.1.3  22.0.4 wheel
      setuptools 56.0.0  62.1.0 wheel
      WARNING: You are using pip version 21.1.3; however, version 22.0.4 is available.
      You should consider upgrading via the '/Users/user/github/scons/work/myvenv/bin/python3 -m pip install --upgrade pip' command.
      (myvenv) user@host Work % pip install --upgrade pip setuptools scons
      Requirement already satisfied: pip in ./myvenv/lib/python3.9/site-packages (21.1.3)
      Collecting pip
        Downloading pip-22.0.4-py3-none-any.whl (2.1 MB)
      Requirement already satisfied: setuptools in ./myvenv/lib/python3.9/site-packages (56.0.0)
      Collecting setuptools
        Downloading setuptools-62.1.0-py3-none-any.whl (1.1 MB)
      Collecting scons
        Downloading SCons-4.3.0-py3-none-any.whl (4.2 MB)
      Installing collected packages: setuptools, scons, pip
        Attempting uninstall: setuptools
          Found existing installation: setuptools 56.0.0
          Uninstalling setuptools-56.0.0:
            Successfully uninstalled setuptools-56.0.0
        Attempting uninstall: pip
          Found existing installation: pip 21.1.3
          Uninstalling pip-21.1.3:
            Successfully uninstalled pip-21.1.3
      Successfully installed pip-22.0.4 scons-4.3.0 setuptools-62.1.0
      user@host Work % scons --version
      SCons by Steven Knight et al.:
              SCons: v4.3.0.559790274f66fa55251f5754de34820a29c7327a, Tue, 16 Nov 2021 19:09:21 +0000, by bdeegan on octodog
              SCons path: ['/Users/user/github/scons/work/myvenv/lib/python3.9/site-packages/SCons']
      Copyright (c) 2001 - 2021 The SCons Foundation
      (myvenv) user@host Work % deactivate
      user@host Work %


To use this virtualenv for work, repeat the
step to activate it, go to your project, and install
any additional requirements your project may have
(skip this step if there are none).

.. tabs::
   .. code-tab:: console Linux

      $ cd Work/myproj
      $ source ~/Work/myvenv/bin/activate
      (myvenv) $ pip install -r requirements.txt

   .. code-tab:: doscon Windows

      C:\Users\user\Work> $ cd Work
      C:\Users\user\Work>.\myvenv\Scripts\activate
      C:\Users\user\Work> $ cd myproj
      (myvenv) C:\Users\user\Work\myproj> pip install -r requirements.txt

   .. code-tab:: console MacOS

      user@host scons % cd Work/myproj
      user@host myproj % source ~/Work/myvenv/bin/activate
      (myvenv) user@host myproj % pip install -r requirements.txt
