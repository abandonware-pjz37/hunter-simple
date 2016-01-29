| Linux                           | Mac                             |
|---------------------------------|---------------------------------|
| [![Build Status][master]][repo] | [![Build Status][macosx]][repo] |

Example of downloading/installing dependencies using [hunter][1] package manager.

### Requirements

* CMake version 3.0

### Usage

* Set `HUNTER_ROOT` environment variable (recommended, see [alternatives][2])
* Run generate: `cmake -H. -B_builds -DHUNTER_STATUS_DEBUG=ON`
* Run build: `cmake --build _builds`
* Run test: `cd _builds && ctest`

### Output

* Download starts in `HUNTER_ROOT` directory:
```bash
-- [hunter] Hunter not found, start download to '/path/to/hunter/root/' ...
```

* After download done cmake variable `HUNTER_ROOT` set:
```bash
-- [hunter] HUNTER_ROOT: /path/to/hunter/root/
```

* Requested package [gtest][3] will be downloaded using wrapped [ExternalProject_Add][4] command:
```
-- [hunter] GTEST_ROOT: /path/to/hunter/root/Base/Install/default (ver.: 1.7.0-hunter-7)
```

### Usage (toolchains)

Build functionality can be extended using [toolchains][5]:

* Download toolchains archive (unix-style):
```
> export POLLY_VERSION="0.4.4"
> wget "https://github.com/ruslo/polly/archive/v${POLLY_VERSION}.tar.gz"
> tar xf "v${POLLY_VERSION}.tar.gz"
> export POLLY_ROOT="`pwd`/polly-${POLLY_VERSION}"
> export PATH="${POLLY_ROOT}/bin:${PATH}"
```
* Each of this toolchain set `POLLY_TOOLCHAIN_TAG` and `HUNTER_INSTALL_TAG` [variables][8]
which can be used to differentiate one build from another

* Now `build.py` python3 script from `${POLLY_ROOT}/bin` directory can be used to start for example `Xcode` project:
```
> build.py --toolchain xcode --open --nobuild --fwd HUNTER_STATUS_DEBUG=ON
```
This script equivalent to:
```
> cmake -H. -B_builds/xcode -DHUNTER_STATUS_DEBUG=ON -DCMAKE_TOOLCHAIN_FILE=${POLLY_ROOT}/xcode.cmake -GXcode
> open _builds/xcode/HunterSimple.xcodeproj
```

Check [gtest][3] directories:
```
/path/to/hunter/root/Base/Install/xcode # install directory
/path/to/hunter/root/Base/Source/GTest # unpacked source directory
/path/to/hunter/root/Download/GTest # path to downloaded archive
```

* *Note*: for building with [ios][6] need to be used patched version of [cmake][7]

### More

* [Travis CI config for Linux](https://github.com/forexample/hunter-simple/blob/master/.travis.yml)
* [Travis CI config for Mac](https://github.com/forexample/hunter-simple/blob/macosx/.travis.yml)
* [Weather (Boost, CppNetlib.URI, GTest, JSON Spirit)](https://github.com/ruslo/weather)

[master]: https://travis-ci.org/forexample/hunter-simple.svg?branch=master
[macosx]: https://travis-ci.org/forexample/hunter-simple.svg?branch=macosx
[repo]: https://travis-ci.org/forexample/hunter-simple
[1]: https://github.com/ruslo/hunter
[2]: https://github.com/hunter-packages/gate#effects
[3]: https://github.com/ruslo/hunter/wiki/Packages#gtest
[4]: http://www.cmake.org/cmake/help/v3.0/module/ExternalProject.html
[5]: https://github.com/ruslo/polly#toolchains
[6]: https://github.com/ruslo/polly/wiki/Toolchain-list#ios
[7]: https://github.com/ruslo/CMake/releases/tag/v3.0.0-ios-universal
[8]: https://github.com/ruslo/polly/blob/98254bccacbf752e4375c71476ba65ab3a1f01d4/utilities/polly_common.cmake#L36
