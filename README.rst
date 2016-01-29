.. |AppVeyor| image:: https://ci.appveyor.com/api/projects/status/ya8pap2nvskth4mx/branch/master?svg=true
  :target: https://ci.appveyor.com/project/ruslo/hunter-simple/branch/master

.. |TravisCI| image:: https://travis-ci.org/forexample/hunter-simple.svg?branch=master
  :target: https://travis-ci.org/forexample/hunter-simple

========== ==========
Linux/OSX  Windows
========== ==========
|TravisCI| |AppVeyor|
========== ==========

Example of downloading/installing dependencies using `Hunter`_ package manager.

Requirements
------------

* `CMake version 3.0 <https://github.com/ruslo/hunter#notes-about-version-of-cmake>`_

Usage
-----

* Set ``HUNTER_ROOT`` environment variable (recommended, see `alternatives`_)
* Run generate: ``cmake -H. -B_builds -DHUNTER_STATUS_DEBUG=ON``
* Run build: ``cmake --build _builds``
* Run test: ``cd _builds && ctest``

Output
------

* Download starts in ``HUNTER_ROOT`` directory:

.. code-block:: bash

  -- [hunter] Hunter not found, start download to '/path/to/hunter/root/' ...

* After download done cmake variable ``HUNTER_ROOT`` set:

.. code-block:: bash

  -- [hunter] HUNTER_ROOT: /path/to/hunter/root/

* Requested package `GTest`_ will be downloaded using wrapped `ExternalProject_Add`_ command:

.. code-block:: bash

  -- [hunter] GTEST_ROOT: /path/to/hunter/root/Base/Install/default (ver.: 1.7.0-hunter-7)

Usage (toolchains)
------------------

Build functionality can be extended using `toolchains`_:

* Download toolchains archive (unix-style):

.. code-block:: bash

  > export POLLY_VERSION="0.4.4"
  > wget "https://github.com/ruslo/polly/archive/v${POLLY_VERSION}.tar.gz"
  > tar xf "v${POLLY_VERSION}.tar.gz"
  > export POLLY_ROOT="`pwd`/polly-${POLLY_VERSION}"
  > export PATH="${POLLY_ROOT}/bin:${PATH}"

* Now ``build.py`` python3 script from ``${POLLY_ROOT}/bin`` directory can be used to start for example ``Xcode`` project:

.. code-block:: bash

  > build.py --toolchain xcode --open --nobuild --fwd HUNTER_STATUS_DEBUG=ON

This script equivalent to:

.. code-block:: bash

  > cmake -H. -B_builds/xcode -DHUNTER_STATUS_DEBUG=ON -DCMAKE_TOOLCHAIN_FILE=${POLLY_ROOT}/xcode.cmake -GXcode
  > open _builds/xcode/HunterSimple.xcodeproj

Check `GTest`_ directories:

.. code-block:: bash

  /path/to/hunter/root/Base/Install/xcode # install directory
  /path/to/hunter/root/Base/Source/GTest # unpacked source directory
  /path/to/hunter/root/Download/GTest # path to downloaded archive

.. node::

  For building with `iOS`_ need to be used patched version of `CMake`_

More
----

* `Travis CI config for Linux/OSX <https://github.com/forexample/hunter-simple/blob/master/.travis.yml>`_
* `AppVeyor config for Windows <https://github.com/forexample/hunter-simple/blob/master/appveyor.yml>`_
* `Weather (Boost, CppNetlib.URI, GTest, JSON Spirit <https://github.com/ruslo/weather>`_

.. _Hunter: https://github.com/ruslo/hunter
.. _alternatives: https://github.com/hunter-packages/gate#effects
.. _GTest: https://github.com/ruslo/hunter/wiki/Packages#gtest
.. _ExternalProject_Add: http://www.cmake.org/cmake/help/v3.0/module/ExternalProject.html
.. _toolchains: https://github.com/ruslo/polly#toolchains

.. _iOS: https://github.com/ruslo/polly/wiki/Toolchain-list#ios
.. _CMake: https://github.com/ruslo/hunter#notes-about-version-of-cmake
