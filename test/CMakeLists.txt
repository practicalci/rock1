# Tests for rock1

# Add here additional source files
add_executable( ${PROJECT_NAME}_test test_rock1.cpp )

# make sure you execute "conan install .." in the build directory
target_link_libraries(${PROJECT_NAME}_test ${PROJECT_NAME}_lib CONAN_PKG::catch2)

# Register the tests
add_test(NAME ${PROJECT_NAME}_test
         COMMAND ${PROJECT_NAME}_test)
