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

