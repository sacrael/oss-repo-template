### Lab 5
# Build Systems

### Cmake Tutorial
#### Step 1 Results

#### **`tutorial.cxx`**
``` cpp
#include <cmath>
#include <iostream>
#include <string>
#include "TutorialConfig.h"

int main(int argc, char* argv[])
{
  if (argc < 2) {
        std::cout << argv[0] << " Version " << Tutorial_VERSION_MAJOR << "." << Tutorial_VERSION_MINOR << std::endl;
    std::cout << "Usage: " << argv[0] << " number" << std::endl;
    return 1;
  }

  // convert input to double
  const double inputValue = std::stod(argv[1]);

  // calculate square root
  const double outputValue = sqrt(inputValue);
  std::cout << "The square root of " << inputValue << " is " << outputValue
            << std::endl;
  return 0;
}
```

#### **`CMakeLists.txt`**
``` cmake
cmake_minimum_required(VERSION 3.10)

project(Tutorial VERSION 1.0)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

configure_file(TutorialConfig.h.in TutorialConfig.h)

add_executable(Tutorial tutorial.cxx)

target_include_directories(Tutorial PUBLIC
    "${PROJECT_BINARY_DIR}"
)
```

#### **Output**
![image](https://user-images.githubusercontent.com/58189969/109440546-a9113d00-7a00-11eb-8278-48229258a777.png)


#### Step 2 Results

#### **`tutorial.cxx`**
``` cpp
// A simple program that computes the square root of a number
#include <cmath>
#include <iostream>
#include <string>

#include "TutorialConfig.h"

#ifdef USE_MYMATH
#include "MathFunctions.h"
#define SQRT_FN mysqrt
#else
#define SQRT_FN sqrt
#endif

int main(int argc, char* argv[])
{
  if (argc < 2) {
    // report version
    std::cout << argv[0] << " Version " << Tutorial_VERSION_MAJOR << "."
              << Tutorial_VERSION_MINOR << std::endl;
    std::cout << "Usage: " << argv[0] << " number" << std::endl;
    return 1;
  }

  // convert input to double
  const double inputValue = std::stod(argv[1]);

  // calculate square root
  const double outputValue = SQRT_FN(inputValue);
  std::cout << "The square root of " << inputValue << " is " << outputValue
            << std::endl;
  return 0;
}
```

#### **`CMakeLists.txt`**
``` cmake
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# ======== BEGIN ADD OPTIONS =============
option(USE_MYMATH "Use tutorial provided math implementation" ON)
# ======== END ADD OPTIONS ===============

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# ========= START ADD MATHFUNCTIONS =============
if(USE_MYMATH)
        add_subdirectory(MathFunctions)
        list(APPEND EXTRA_LIBS MathFunctions)
        list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")
endif()
add_executable(Tutorial tutorial.cxx) # add the executable
target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})
# ========= END ADD MATH FUNCTIONS ==============

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                                                   ${EXTRA_INCLUDES}
                                                   )
```

#### **Output**
![image](https://user-images.githubusercontent.com/58189969/109442175-7158c400-7a05-11eb-80e9-22f6819e1a2c.png)

#### Step 3 Results
#### **`MathFunctions/CMakeList.txt`**
``` cmake
add_library(MathFunctions mysqrt.cxx)

target_include_directories(MathFunctions
                  INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
```

#### **`CMakeList.txt`**
``` cmake
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )
```

#### **Output**
![image](https://user-images.githubusercontent.com/58189969/109442515-7407e900-7a06-11eb-8945-9a089556cb1d.png)

#### Step 4 Results
#### **`MathFunctions/CMakeLists.txt`**
``` cpp
add_library(MathFunctions mysqrt.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
          )

install(TARGETS MathFunctions DESTINATION lib)
install(FILES MathFunctions.h DESTINATION include)
```

#### **`CMakeLists.txt`**
``` cpp
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )

install(TARGETS Tutorial DESTINATION bin)
install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h"
          DESTINATION include
            )


enable_testing()

# does the application run
add_test(NAME Runs COMMAND Tutorial 25)

# does the usage message work?
add_test(NAME Usage COMMAND Tutorial)
set_tests_properties(Usage
        PROPERTIES PASS_REGULAR_EXPRESSION "Usage:.*number"
)

# define a function to simplify adding tests
function(do_test target arg result)
add_test(NAME Comp${arg} COMMAND ${target} ${arg})
set_tests_properties(Comp${arg}
        PROPERTIES PASS_REGULAR_EXPRESSION ${result}
)
endfunction(do_test)

# do a bunch of result based tests
do_test(Tutorial 4 "4 is 2")
do_test(Tutorial 9 "9 is 3")
do_test(Tutorial 5 "5 is 2.236")
do_test(Tutorial 7 "7 is 2.645")
do_test(Tutorial 25 "25 is 5")
do_test(Tutorial -25 "-25 is [-nan|nan|0]")
do_test(Tutorial 0.0001 "0.0001 is 0.01")
```

#### **Output**
![image](https://user-images.githubusercontent.com/58189969/109443062-9bab8100-7a07-11eb-95de-6c06c9c24af6.png)

#### Step 5 Result

#### **CMakeLists.txt**
``` cmake
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)
target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )

# add the install targets
install(TARGETS Tutorial DESTINATION bin)
install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h"
  DESTINATION include
  )

# enable testing
enable_testing()

# does the application run
add_test(NAME Runs COMMAND Tutorial 25)

# does the usage message work?
add_test(NAME Usage COMMAND Tutorial)
set_tests_properties(Usage
  PROPERTIES PASS_REGULAR_EXPRESSION "Usage:.*number"
  )

# define a function to simplify adding tests
function(do_test target arg result)
  add_test(NAME Comp${arg} COMMAND ${target} ${arg})
  set_tests_properties(Comp${arg}
    PROPERTIES PASS_REGULAR_EXPRESSION ${result}
    )
endfunction(do_test)

# do a bunch of result based tests
do_test(Tutorial 4 "4 is 2")
do_test(Tutorial 9 "9 is 3")
do_test(Tutorial 5 "5 is 2.236")
do_test(Tutorial 7 "7 is 2.645")
do_test(Tutorial 25 "25 is 5")
do_test(Tutorial -25 "-25 is [-nan|nan|0]")
do_test(Tutorial 0.0001 "0.0001 is 0.01")
```

#### **MathFunctions/CMakeLists.txt**
``` cmake
add_library(MathFunctions mysqrt.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
          )

# install rules
install(TARGETS MathFunctions DESTINATION lib)
install(FILES MathFunctions.h DESTINATION include)

include(CheckSymbolExists)
check_symbol_exists(log "math.h" HAVE_LOG)
check_symbol_exists(exp "math.h" HAVE_EXP)
if(NOT (HAVE_LOG AND HAVE_EXP))
        unset(HAVE_LOG CACHE)
        unset(HAVE_EXP CACHE)
        set(CMAKE_REQUIRED_LIBRARIES "m")
        check_symbol_exists(log "math.h" HAVE_LOG)
        check_symbol_exists(exp "math.h" HAVE_EXP)
        if(HAVE_LOG AND HAVE_EXP)
                target_link_libraries(MathFunctions PRIVATE m)
        endif()
endif()

if(HAVE_LOG AND HAVE_EXP)
        target_compile_definitions(MathFunctions
                                       PRIVATE "HAVE_LOG" "HAVE_EXP")
endif()
```

#### **Output**
![image](https://user-images.githubusercontent.com/58189969/109443593-eda0d680-7a08-11eb-9235-66e5dd428dc9.png)

#### Program Building

#### **`source/Makefile`**
``` make
static: block.c
        gcc -c -o block.o block.c
        ar rcs block.a block.o


shared: block.c
        gcc -c -fPIC block.c -o block.o
        gcc block.o -shared -o libblock.so
```

#### **`CMakeLists.txt`**
``` cmake
cmake_minimum_required(VERSION 3.10)

project(Program VERSION 1.0)

add_library(block STATIC source/block.c)
target_compile_definitions(block PUBLIC block)

add_library(blockshr SHARED source/block.c)
target_compile_definitions(blockshr PUBLIC blockshr)

add_executable(program program.c)
target_link_libraries(program block)

add_executable(programshr program.c)
target_link_libraries(programshr blockshr)
```

#### **`Build/Makefile`**
``` make
# CMAKE generated file: DO NOT EDIT!
# Generated by "Unix Makefiles" Generator, CMake Version 3.17

# Default target executed when no arguments are given to make.
default_target: all

.PHONY : default_target

# Allow only one "make -f Makefile2" at a time, but pass parallelism.
.NOTPARALLEL:


#=============================================================================
# Special targets provided by cmake.

# Disable implicit rules so canonical targets will work.
.SUFFIXES:


# Disable VCS-based implicit rules.
% : %,v


# Disable VCS-based implicit rules.
% : RCS/%


# Disable VCS-based implicit rules.
% : RCS/%,v


# Disable VCS-based implicit rules.
% : SCCS/s.%


# Disable VCS-based implicit rules.
% : s.%


.SUFFIXES: .hpux_make_needs_suffix_list


# Command-line flag to silence nested $(MAKE).
$(VERBOSE)MAKESILENT = -s

# Suppress display of executed commands.
$(VERBOSE).SILENT:


# A target that is always out of date.
cmake_force:

.PHONY : cmake_force

#=============================================================================
# Set environment variables for the build.

# The shell in which to execute make rules.
SHELL = /bin/sh

# The CMake executable.
CMAKE_COMMAND = /usr/local/bin/cmake

# The command to remove a file.
RM = /usr/local/bin/cmake -E rm -f

# Escaping for special characters.
EQUALS = =

# The top-level source directory on which CMake was run.
CMAKE_SOURCE_DIR = /mnt/c/Users/Muiz/Documents/RPI/CSCI-4470-OpenSource/Modules/05.BuildSystems/Lab-BuildSystemsExample

# The top-level build directory on which CMake was run.
CMAKE_BINARY_DIR = /mnt/c/Users/Muiz/Documents/RPI/CSCI-4470-OpenSource/Modules/05.BuildSystems/Lab-BuildSystemsExample/Build

#=============================================================================
# Targets provided globally by CMake.

# Special rule for the target rebuild_cache
rebuild_cache:
        @$(CMAKE_COMMAND) -E cmake_echo_color --switch=$(COLOR) --cyan "Running CMake to regenerate build system..."
        /usr/local/bin/cmake --regenerate-during-build -S$(CMAKE_SOURCE_DIR) -B$(CMAKE_BINARY_DIR)
.PHONY : rebuild_cache

# Special rule for the target rebuild_cache
rebuild_cache/fast: rebuild_cache

.PHONY : rebuild_cache/fast

# Special rule for the target edit_cache
edit_cache:
        @$(CMAKE_COMMAND) -E cmake_echo_color --switch=$(COLOR) --cyan "Running CMake cache editor..."
        /usr/local/bin/ccmake -S$(CMAKE_SOURCE_DIR) -B$(CMAKE_BINARY_DIR)
.PHONY : edit_cache

# Special rule for the target edit_cache
edit_cache/fast: edit_cache

.PHONY : edit_cache/fast

# The main all target
all: cmake_check_build_system
        $(CMAKE_COMMAND) -E cmake_progress_start /mnt/c/Users/Muiz/Documents/RPI/CSCI-4470-OpenSource/Modules/05.BuildSystems/Lab-BuildSystemsExample/Build/CMakeFiles /mnt/c/Users/Muiz/Documents/RPI/CSCI-4470-OpenSource/Modules/05.BuildSystems/Lab-BuildSystemsExample/Build/CMakeFiles/progress.marks
        $(MAKE) $(MAKESILENT) -f CMakeFiles/Makefile2 all
        $(CMAKE_COMMAND) -E cmake_progress_start /mnt/c/Users/Muiz/Documents/RPI/CSCI-4470-OpenSource/Modules/05.BuildSystems/Lab-BuildSystemsExample/Build/CMakeFiles 0
.PHONY : all

# The main clean target
clean:
        $(MAKE) $(MAKESILENT) -f CMakeFiles/Makefile2 clean
.PHONY : clean

# The main clean target
clean/fast: clean

.PHONY : clean/fast

# Prepare targets for installation.
preinstall: all
        $(MAKE) $(MAKESILENT) -f CMakeFiles/Makefile2 preinstall
.PHONY : preinstall

# Prepare targets for installation.
preinstall/fast:
        $(MAKE) $(MAKESILENT) -f CMakeFiles/Makefile2 preinstall
.PHONY : preinstall/fast

# clear depends
depend:
        $(CMAKE_COMMAND) -S$(CMAKE_SOURCE_DIR) -B$(CMAKE_BINARY_DIR) --check-build-system CMakeFiles/Makefile.cmake 1
.PHONY : depend

#=============================================================================
# Target rules for targets named programshr

# Build rule for target.
programshr: cmake_check_build_system
        $(MAKE) $(MAKESILENT) -f CMakeFiles/Makefile2 programshr
.PHONY : programshr

# fast build rule for target.
programshr/fast:
        $(MAKE) $(MAKESILENT) -f CMakeFiles/programshr.dir/build.make CMakeFiles/programshr.dir/build
.PHONY : programshr/fast

#=============================================================================
# Target rules for targets named program

# Build rule for target.
program: cmake_check_build_system
        $(MAKE) $(MAKESILENT) -f CMakeFiles/Makefile2 program
.PHONY : program

# fast build rule for target.
program/fast:
        $(MAKE) $(MAKESILENT) -f CMakeFiles/program.dir/build.make CMakeFiles/program.dir/build
.PHONY : program/fast

#=============================================================================
# Target rules for targets named blockshr

# Build rule for target.
blockshr: cmake_check_build_system
        $(MAKE) $(MAKESILENT) -f CMakeFiles/Makefile2 blockshr
.PHONY : blockshr

# fast build rule for target.
blockshr/fast:
        $(MAKE) $(MAKESILENT) -f CMakeFiles/blockshr.dir/build.make CMakeFiles/blockshr.dir/build
.PHONY : blockshr/fast

#=============================================================================
# Target rules for targets named block

# Build rule for target.
block: cmake_check_build_system
        $(MAKE) $(MAKESILENT) -f CMakeFiles/Makefile2 block
.PHONY : block

# fast build rule for target.
block/fast:
        $(MAKE) $(MAKESILENT) -f CMakeFiles/block.dir/build.make CMakeFiles/block.dir/build
.PHONY : block/fast

program.o: program.c.o

.PHONY : program.o

# target to build an object file
program.c.o:
        $(MAKE) $(MAKESILENT) -f CMakeFiles/programshr.dir/build.make CMakeFiles/programshr.dir/program.c.o
        $(MAKE) $(MAKESILENT) -f CMakeFiles/program.dir/build.make CMakeFiles/program.dir/program.c.o
.PHONY : program.c.o

program.i: program.c.i

.PHONY : program.i

# target to preprocess a source file
program.c.i:
        $(MAKE) $(MAKESILENT) -f CMakeFiles/programshr.dir/build.make CMakeFiles/programshr.dir/program.c.i
        $(MAKE) $(MAKESILENT) -f CMakeFiles/program.dir/build.make CMakeFiles/program.dir/program.c.i
.PHONY : program.c.i

program.s: program.c.s

.PHONY : program.s

# target to generate assembly for a file
program.c.s:
        $(MAKE) $(MAKESILENT) -f CMakeFiles/programshr.dir/build.make CMakeFiles/programshr.dir/program.c.s
        $(MAKE) $(MAKESILENT) -f CMakeFiles/program.dir/build.make CMakeFiles/program.dir/program.c.s
.PHONY : program.c.s

source/block.o: source/block.c.o

.PHONY : source/block.o

# target to build an object file
source/block.c.o:
        $(MAKE) $(MAKESILENT) -f CMakeFiles/blockshr.dir/build.make CMakeFiles/blockshr.dir/source/block.c.o
        $(MAKE) $(MAKESILENT) -f CMakeFiles/block.dir/build.make CMakeFiles/block.dir/source/block.c.o
.PHONY : source/block.c.o

source/block.i: source/block.c.i

.PHONY : source/block.i

# target to preprocess a source file
source/block.c.i:
        $(MAKE) $(MAKESILENT) -f CMakeFiles/blockshr.dir/build.make CMakeFiles/blockshr.dir/source/block.c.i
        $(MAKE) $(MAKESILENT) -f CMakeFiles/block.dir/build.make CMakeFiles/block.dir/source/block.c.i
.PHONY : source/block.c.i

source/block.s: source/block.c.s

.PHONY : source/block.s

# target to generate assembly for a file
source/block.c.s:
        $(MAKE) $(MAKESILENT) -f CMakeFiles/blockshr.dir/build.make CMakeFiles/blockshr.dir/source/block.c.s
        $(MAKE) $(MAKESILENT) -f CMakeFiles/block.dir/build.make CMakeFiles/block.dir/source/block.c.s
.PHONY : source/block.c.s

# Help Target
help:
        @echo "The following are some of the valid targets for this Makefile:"
        @echo "... all (the default if no target is provided)"
        @echo "... clean"
        @echo "... depend"
        @echo "... edit_cache"
        @echo "... rebuild_cache"
        @echo "... block"
        @echo "... blockshr"
        @echo "... program"
        @echo "... programshr"
        @echo "... program.o"
        @echo "... program.i"
        @echo "... program.s"
        @echo "... source/block.o"
        @echo "... source/block.i"
        @echo "... source/block.s"
.PHONY : help



#=============================================================================
# Special targets to cleanup operation of make.

# Special rule to run CMake to check the build system integrity.
# No rule that depends on this can have commands that come from listfiles
# because they might be regenerated.
cmake_check_build_system:
        $(CMAKE_COMMAND) -S$(CMAKE_SOURCE_DIR) -B$(CMAKE_BINARY_DIR) --check-build-system CMakeFiles/Makefile.cmake 0
.PHONY : cmake_check_build_system
```

#### **Lib File Difference Output**
![image](https://user-images.githubusercontent.com/58189969/109446767-49229280-7a10-11eb-818e-e743376d7632.png)
