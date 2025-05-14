cmake_minimum_required(VERSION 3.27)

include(FetchContent)

# Fetch Google Test
FetchContent_Declare(
  googletest
  GIT_REPOSITORY https://github.com/google/googletest.git
  GIT_TAG        release-1.11.0
)
FetchContent_MakeAvailable(googletest)

# Set the runtime library for Google Test
if (MSVC)
    target_compile_options(gtest PRIVATE "/MD$<$<CONFIG:Debug>:d>")
    target_compile_options(gtest_main PRIVATE "/MD$<$<CONFIG:Debug>:d>")
endif()

# Define dummy_test executable
add_executable(dummy_test
    test.cpp
)

# Specify the runtime library for the executable
if (MSVC)
    target_compile_options(dummy_test PRIVATE "/MD$<$<CONFIG:Debug>:d>")
endif()

# Link libraries to dummy_test
target_link_libraries(dummy_test
    PUBLIC
    gtest_main
    example
)

enable_testing()

add_test(example_gtests dummy_test)

# find_program(OPENCPPCOVERAGE_COMMAND OpenCppCoverage)
# if(NOT OPENCPPCOVERAGE_COMMAND)
#     message(FATAL_ERROR "OpenCppCoverage not found.")
# endif()

# # Command to run tests with OpenCppCoverage
# add_custom_target(run_tests_with_coverage
#     COMMAND ${OPENCPPCOVERAGE_COMMAND} --sources "path to Module to compile" -- "add .exe path"
#     DEPENDS dummy_test
#     COMMENT "Running tests with code coverage..."
# )

# # Add coverage target
# add_custom_target(coverage
#     COMMAND ${CMAKE_COMMAND} -E copy_directory "${CMAKE_BINARY_DIR}/coverage_report" "${CMAKE_BINARY_DIR}/previous_coverage_report"
#     COMMAND ${OPENCPPCOVERAGE_COMMAND} --export_type html --output "${CMAKE_BINARY_DIR}/coverage_report"
#     DEPENDS run_tests_with_coverage
#     COMMENT "Generating code coverage report..."
# )

