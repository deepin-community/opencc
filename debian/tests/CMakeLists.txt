cmake_minimum_required(VERSION 3.18)
project (opencc-integration CXX)
include (CTest)
enable_testing()

find_package(PkgConfig REQUIRED)
find_program (OPENCC_TOOL opencc REQUIRED)
pkg_check_modules(OPENCC REQUIRED IMPORTED_TARGET opencc)

add_definitions(
  -DCMAKE_BINARY_DIR="${CMAKE_BINARY_DIR}"
  -DCMAKE_SOURCE_DIR="${CMAKE_SOURCE_DIR}"
  -DPROJECT_BINARY_PATH="${OPENCC_TOOL}"
)
make_directory(${CMAKE_BINARY_DIR}/test)
add_subdirectory(/usr/src/googletest/googletest ${CMAKE_BINARY_DIR}/googletest-build EXCLUDE_FROM_ALL)
set(UNITTESTS
  CommandLineConvertTest
)
foreach(UNITTEST ${UNITTESTS})
  add_executable(${UNITTEST} ${UNITTEST}.cpp)
  target_link_libraries(${UNITTEST} gtest gtest_main PkgConfig::OPENCC)
  add_test(${UNITTEST} ${UNITTEST})
endforeach(UNITTEST)
