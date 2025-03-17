cmake_minimum_required(VERSION 3.14)
project(jostl LANGUAGES CXX)

# Set C++ standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
# set(CMAKE_CXX_EXTENSIONS OFF)

# Define the library
add_library(jostl STATIC
    src/vector.hpp
    src/list.hpp
    src/map.hpp
    # Add other source files as needed
)

# Set include directories
target_include_directories(jostl PUBLIC include)

# Enable position-independent code (important for shared libraries)
set_target_properties(jostl PROPERTIES POSITION_INDEPENDENT_CODE ON)

# Install targets for use in other projects
install(TARGETS jostl EXPORT jostlConfig
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION bin
)

install(DIRECTORY include/ DESTINATION include)

install(EXPORT jostlConfig
    FILE jostlConfig.cmake
    NAMESPACE jostl::
    DESTINATION lib/cmake/jostl
)

include(FetchContent)

FetchContent_Declare(
    googletest
    GIT_REPOSITORY https://github.com/google/googletest.git
    GIT_TAG 6910c9d9165801d8827d628cb72eb7ea9dd538c5
)

set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

enable_testing()

add_executable(test_jostl
    tests/vector_test.cpp
    tests/list_test.cpp
)

target_link_libraries(test_jostl PRIVATE jostl GTest::gtest GTest::gtest_main)
