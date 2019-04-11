[![Build Status](https://travis-ci.com/TheLartians/CPM.svg?branch=master)](https://travis-ci.com/TheLartians/CPM)

# CPM

CPM is a minimalistic package manager written in Cmake using `find_package` and `FetchContent` as a fallback to download non locally installed packages.

# Usage

To add a new dependency to your project simply add the Projects target name, the git URL and the version. If the git tag for this version does not match the pattern `v$VERSION`, then the exact branch or tag can be specified with the `GIT_TAG` argument.

```cmake
cmake_minimum_required(VERSION 3.14 FATAL_ERROR)

project(MyParser)

# add dependencies
include(${CMAKE_CURRENT_SOURCE_DIR}/cmake/CPM.cmake)

CPMAddPackage(
  NAME LarsParser
  VERSION 1.4 # optional, used for find_package
  GIT_REPOSITORY https://github.com/TheLartians/Parser.git
  GIT_TAG v1.4 # optional if TAG matches v$VERSION
)

# add executable
set (CMAKE_CXX_STANDARD 17)
add_executable(my-parser my-parser.cpp)
target_link_libraries(cpm-test LarsParser)
```

# Adding CPM

To add CPM to your current project, copy the scripts in the `cmake` directory into you current project project. The command below will perform this automatically.

```bash
wget -O cmake/CPM.cmake https://raw.githubusercontent.com/TheLartians/CPM/master/cmake/CPM.cmake
```

# Advantages

- **Auto handle dependencies** Users of your projects do not need to worry about dependencies, everything is handled automatically.
- **Reproducable builds** Using git tags it is ensured that a project will always be in the same state everywhere.
- **No installation required** No need to install any third-party package managers. Just copy the files from the CMake directory and you're good to go.
- **Cross-Plattform** As CPM adds projects as cmake subdirectories, it is compatible with all cmake toolchains and generators. 

# Limitations

- **First version used** In diamond-shaped dependency graphs (e.g. `A` depends on `C`(v1.1) and `A` depends on `B` depends on `C`(v1.2)) the first added dependency will be used (in this case `C`@1.1).
- **No auto-update** To update a dependency, version numbers or git tags in the cmake scripts must be adapted manually.
