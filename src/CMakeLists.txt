# Target:   rock1_lib
# Type:     Library
file(GLOB SOURCE *.cpp)
add_library(rock1_lib SHARED ${SOURCE})
set_target_properties(rock1_lib PROPERTIES
    "OUTPUT_NAME" "rock1"
    "INCLUDE_DIRECTORIES" "${CMAKE_CURRENT_SOURCE_DIR}/include"
)
install(TARGETS rock1_lib EXPORT rock1 DESTINATION ${CMAKE_SOURCE_DIR}/lib)
export(TARGETS rock1_lib FILE ${CMAKE_BINARY_DIR}/rock1-config.cmake)
