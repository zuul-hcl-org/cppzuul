cppbase
===

<a href="http://opensource.org/licenses/MIT" target="_blank"><img src="http://img.shields.io/badge/license-MIT-brightgreen.png" alt="MIT license"></a><a href="https://travis-ci.org/kartikkumar/cppbase" target="_blank"><img src="https://travis-ci.org/kartikkumar/cppbase.png?branch=master" alt="Build Status"></a><a href="https://scan.coverity.com/projects/3686" target="_blank"><img src="https://img.shields.io/coverity/scan/3686.png" alt="Coverity Scan Build Status"></a><a href="https://coveralls.io/github/kartikkumar/cppbase?branch=master" target="_blank"><img src="https://coveralls.io/repos/github/kartikkumar/cppbase/badge.png?branch=master" alt="Coverage Status"></a>

`cppbase` is a template for a simple CMake-based C++ project: it can be used as the basis for new projects. An example CMake module is available to make it easy to include your project in other projects (should be customized): [Findcppbase.cmake](https://github.com/openastro/cmake-modules/blob/master/Modules/Findcppbase.cmake).

(**N.B. remember to change all instances of `cppbase` to your new project name!**).

Features
------

  - General directory structure common to C++ projects
  - Example of CMake module [Findcppbase.cmake](https://github.com/openastro/cmake-modules/blob/master/Modules/Findcppbase.cmake)
  - Unit testing framework ([Catch2](https://www.github.com/catchorg/Catch2 "Catch2 Github repository") or [GTest](https://github.com/google/googletest "Google Test Github repository"))
  - Install script (`make install`)
  - Uninstall script (`make uninstall`)
  - CPack script for packaging (`make package`)
  - Automatic API documentation ([Doxygen](http://www.doxygen.org "Doxygen homepage"))
  - Continuous Integration ([Travis CI](https://travis-ci.org/ "Travis CI homepage"))
  - Code coverage analysis ([Coveralls](https://coveralls.io "Coveralls.io homepage")) (`make coverage`)
  - Example of how to include external dependencies (using `ExternalProject` module)
  - Separate file to specify location of project files (`ProjectFiles.cmake`)

Requirements
------

To install this project, please ensure that you have installed the following (install guides are provided on the respective websites):

  - [Git](http://git-scm.com)
  - A C++ compiler, e.g., [GCC](https://gcc.gnu.org/), [clang](http://clang.llvm.org/), [MinGW](http://www.mingw.org/)
  - [CMake](http://www.cmake.org "CMake homepage")
  - [Doxygen](http://www.doxygen.org "Doxygen homepage") (optional)
  - [Gcov](https://gcc.gnu.org/onlinedocs/gcc/Gcov.html) (optional)
  - [LCOV](http://ltp.sourceforge.net/coverage/lcov.php) (optional)

The following dependencies are optional (see `Build options`):

  - [Catch2](https://www.github.com/catchorg/Catch2) (unit testing library necessary for `BUILD_TESTING` build option, with `TEST_FRAMEWORK=Catch2` (default))
  - [GTest](https://github.com/google/googletest) (alternate unit testing library necessary for `BUILD_TESTING` build option and `TEST_FRAMEWORK=GTest`)

These dependencies will be downloaded and configured automagically, if not already present locally (requires an internet connection).

Installation
------

Run the following commands to download, build, and install this project. Substitute `project_name` with the name of your project (if you leave it out, the repository will be cloned to a local folder called `cppbase`). This will customize the project targets for you. Note that `project_name` must not contain spaces! The ` --depth 1` parameter passed to `git clone` ensures that the git history is not downloaded. In case you would like to preserve the history of this project, omit that option.

    git clone https://www.github.com/kartikkumar/cppbase --depth 1 <project_name>
    cd <project_name>
    git submodule init && git submodule update
    mkdir build && cd build
    cmake -DPROJECT_NAME=<project_name> .. && cmake --build .

To push this project to your own remote repository, you can run the following command, which will overcome the issues with utilizing a shallow clone:

    git commit --amend .

This rewrites the last commit and ensures that you can then push the repository to a remote (e.g., Github, BitBucket, Gitlab, etc.).

To install the header files and libraries, run the following from within the `build` directory:

    make install

Note that dependencies are installed by fetching them online, in case they cannot be detected on your local system. If the build process fails, check the error log given. Typically, building fails due to timeout. Simply run the `cmake --build .` command once more.

To uninstall all the files that were installed on your system, run the following from within the `build` directory:

    make uninstall

Note that this does not remove any dependencies that were automagically fetched and built.

Build options
-------------

You can pass the following, general command-line options when running CMake:

  - `-DPROJECT_SUMMARY`: set short string summary for your project
  - `-DPROJECT_VENDOR_NAME`: set short string name for vendor of your project
  - `-DPROJECT_VENDOR_CONTACT`: set short string email address for vendor of your project
  - `-DCMAKE_INSTALL_PREFIX[=$install_dir]`: set path prefix for install script (`make install`); if not set, defaults to usual locations
  - `-DBUILD_SHARED_LIBS=[ON|OFF (default)]`: build shared libraries instead of static
  - `-DBUILD_MAIN[=ON|OFF (default)]`: build the main-function
  - `-DBUILD_DOXYGEN_DOCS[=ON|OFF (default)]`: build the [Doxygen](http://www.doxygen.org "Doxygen homepage") documentation ([LaTeX](http://www.latex-project.org/) must be installed with `amsmath` package)
  - `-DBUILD_TESTING[=ON (default)|OFF]`: build tests (execute tests from build-directory using `ctest -V`)
  - `-DBUILD_DEPENDENCIES[=ON|OFF (default)]`: force local build of dependencies, instead of first searching system-wide using `find_package()`

The following commands are conditional and are only relevant if `BUILD_TESTING = ON`:

 - `-DBUILD_COVERAGE_ANALYSIS[=ON|OFF (default)]`: build code coverage using [Gcov](https://gcc.gnu.org/onlinedocs/gcc/Gcov.html) and [LCOV](http://ltp.sourceforge.net/coverage/lcov.php) (both must be installed; requires [GCC](https://gcc.gnu.org/) compiler; execute coverage analysis from build-directory using `make coverage`)
 - `-DTEST_FRAMEWORK=[=GTest|Catch2 (default)]`: set unit testting framework for your project

Pass these options either directly to the `cmake ..` command, e.g., to build the tests:

    cmake -DPROJECT_NAME=<project_name> -DBUILD_TESTING=on ..

N.B.: Toggling options to build tests using `ccmake` does not work correctly, as the necessarily libraries are not downloaded automagically!

Project structure
-------------

This project has been set up with a specific file/folder structure in mind. The following describes some important features of this setup:

  - `cmake/Modules` : Contains `CMake` modules
  - `docs`: Contains code documentation generated by [Doxygen](http://www.doxygen.org "Doxygen homepage")
  - `include`: Project header files (*.hpp)
  - `scripts`: Shell scripts used in [Travis CI](https://travis-ci.org/ "Travis CI homepage") build
  - `src`: Project source files (*.cpp), including `main.cpp`, which contains example main-function for project build
  - `test`: Project test source files (*.cpp) that are provided to the selecting unit testing framework
  - `.travis.yml`: Configuration file for [Travis CI](https://travis-ci.org/ "Travis CI homepage") build, including static analysis using [Coverity Scan](https://scan.coverity.com/ "Coverity Scan homepage") and code coverage using [Coveralls](https://coveralls.io "Coveralls.io homepage")
  - `CMakeLists.txt`: main `CMakelists.txt` file for project (should not need to be modified for basic build)
  - `Dependencies.cmake`: list of dependencies and automated build, triggered if dependency cannot be found locally
  - `Doxyfile.in`: [Doxygen](http://www.doxygen.org "Doxygen homepage") configuration file, adapted for generic use within project build (should not need to be modified)
  - `LICENSE.md`: license file for project (copyright statement needs to be edited)
  - `ProjectFiles.cmake`: list of project source files to build
  - `Uninstall.cmake.in`: uninstall configuration scrip, necessary for the `make uninstall` target


Contributing
------------

Once you've made your great commits:

  1. [Fork](https://github.com/kartikkumar/cppbase/fork) cppbase
  2. Create a topic branch - `git checkout -b my_branch`
  3. Push to your branch - `git push origin my_branch`
  4. Create a [Pull Request](http://help.github.com/pull-requests/) from your branch
  5. That's it!

Disclaimer
------------

The copyright holders are not liable for any damage(s) incurred due to improper use of `cppbase`.
