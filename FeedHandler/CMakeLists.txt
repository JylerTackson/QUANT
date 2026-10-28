cmake_minimum_required(VERSION 3.20)
project(FeedHandler CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)   # gives VS Code accurate IntelliSense

# Your parser library. INTERFACE = header-only for now; becomes a real lib later.
add_library(itch INTERFACE)
target_include_directories(itch INTERFACE include)
target_compile_options(itch INTERFACE -Wall -Wextra -Wpedantic -Wconversion)

# Debug builds run with AddressSanitizer + UndefinedBehaviorSanitizer.
# These catch out-of-bounds reads and bad shifts at runtime instead of letting them pass silently.
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
  add_compile_options(-fsanitize=address,undefined -fno-omit-frame-pointer)
  add_link_options(-fsanitize=address,undefined)
endif()

# GoogleTest, downloaded at configure time (pattern from the official quickstart)
include(FetchContent)
FetchContent_Declare(googletest
  URL https://github.com/google/googletest/archive/refs/tags/v1.17.0.zip)
FetchContent_MakeAvailable(googletest)

enable_testing()
add_executable(itch_tests tests/test_endian.cpp)
target_link_libraries(itch_tests PRIVATE itch GTest::gtest_main)
include(GoogleTest)
gtest_discover_tests(itch_tests)