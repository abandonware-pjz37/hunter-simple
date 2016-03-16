.. |AppVeyor| image:: https://ci.appveyor.com/api/projects/status/ya8pap2nvskth4mx/branch/master?svg=true
  :target: https://ci.appveyor.com/project/ruslo/hunter-simple/history

.. |TravisCI| image:: https://travis-ci.org/forexample/hunter-simple.svg?branch=master
  :target: https://travis-ci.org/forexample/hunter-simple/builds

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
* Generate project: ``cmake -H. -B_builds -DHUNTER_STATUS_DEBUG=ON -DCMAKE_BUILD_TYPE=Debug``
* Run build: ``cmake --build _builds --config Debug``
* Run test: ``cd _builds && ctest -C Debug -VV``

Output
------

* Download starts in ``HUNTER_ROOT`` directory:

.. code-block:: bash

  -- [hunter] Initializing Hunter workspace (0ccc3f3fd571676a1804723984598f9f90a4d6bc)
  -- [hunter]   https://github.com/ruslo/hunter/archive/v0.12.40.tar.gz
  -- [hunter]   -> /home/travis/.hunter/_Base/Download/Hunter/0.12.40/0ccc3f3

* After download done cmake variable ``HUNTER_ROOT`` set:

.. code-block:: bash

  -- [hunter] HUNTER_ROOT: /home/travis/.hunter

* Requested package `GTest`_ will be downloaded using wrapped `ExternalProject_Add`_ command:

.. code-block:: bash

  -- [hunter] GTEST_ROOT: /home/travis/.hunter/_Base/0ccc3f3/b874e31/3ef6df8/Install (ver.: 1.7.0-hunter-11)

* If local configuration match binary cache from server then it will be used without building from sources:

.. code-block:: bash

  https://raw.githubusercontent.com/ingenue/hunter-cache/.../cache.sha1
    -> /home/travis/.hunter/_Base/Cache/meta/.../cache.sha1
  Cache HIT: GTest
  https://github.com/ingenue/hunter-cache/.../da62fc35901e07d30db7a1c19b7358855978e11f.tar.bz2
    -> /home/travis/.hunter/_Base/Cache/raw/da62fc35901e07d30db7a1c19b7358855978e11f.tar.bz2

Note: since cache uploaded from Travis/AppVeyor CI hence configuration will always match.

Usage (toolchains)
------------------

Build functionality can be extended using `toolchains`_:

* Download toolchains archive (unix-style):

.. code-block:: bash

  > export POLLY_VERSION="0.9.0"
  > wget "https://github.com/ruslo/polly/archive/v${POLLY_VERSION}.tar.gz"
  > tar xf "v${POLLY_VERSION}.tar.gz"
  > export POLLY_ROOT="`pwd`/polly-${POLLY_VERSION}" # not needed in general
  > export PATH="${POLLY_ROOT}/bin:${PATH}"

* Now ``build.py`` python3 script from ``${POLLY_ROOT}/bin`` directory can be used to start for example ``Xcode`` project:

.. code-block:: bash

  > build.py --toolchain xcode --open --nobuild --verbose

This script equivalent to:

.. code-block:: bash

  > cmake -H. -B_builds/xcode -DHUNTER_STATUS_DEBUG=ON -DCMAKE_TOOLCHAIN_FILE=${POLLY_ROOT}/xcode.cmake -GXcode
  > open _builds/xcode/HunterSimple.xcodeproj

Check `GTest`_ directories:

.. code-block:: bash

  /home/travis/.hunter/_Base/0ccc3f3/b874e31/3ef6df8/Install # install directory
  /home/travis/.hunter/_Base/Download/GTest/1.7.0-hunter-11/c6ae948/ # path to downloaded archive

.. node::

  For building with `iOS`_ need to be used patched version of `CMake`_

More
----

* `Travis CI config for Linux/OSX <https://github.com/forexample/hunter-simple/blob/master/.travis.yml>`_
* `AppVeyor config for Windows <https://github.com/forexample/hunter-simple/blob/master/appveyor.yml>`_
* `Weather (Boost, CppNetlib.URI, GTest, JSON Spirit) <https://github.com/ruslo/weather>`_

.. _Hunter: https://github.com/ruslo/hunter
.. _alternatives: https://github.com/hunter-packages/gate#effects
.. _GTest: https://github.com/ruslo/hunter/wiki/pkg.gtest
.. _ExternalProject_Add: http://www.cmake.org/cmake/help/v3.0/module/ExternalProject.html
.. _toolchains: https://github.com/ruslo/polly#toolchains

.. _iOS: https://github.com/ruslo/polly/wiki/Toolchain-list#ios
.. _CMake: https://github.com/ruslo/hunter#notes-about-version-of-cmake
