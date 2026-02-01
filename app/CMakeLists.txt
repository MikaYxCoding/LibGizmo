# Projects Settings
cmake_minimum_required (VERSION 3.10)
project(example)

# The main executable
add_executable(${PROJECT_NAME}
  example.cpp
)
set_target_properties(${PROJECT_NAME} PROPERTIES EXPORT_COMPILE_COMMANDS ON)
if (NOT MSVC)
target_compile_options(${PROJECT_NAME} PRIVATE -Wall)
else()
target_compile_options(${PROJECT_NAME} PRIVATE /W4)
endif()

target_link_libraries(${PROJECT_NAME}
  PRIVATE glfw
  PRIVATE libglew_static
  PRIVATE glm
  PRIVATE libgizmo
)

target_include_directories(${PROJECT_NAME}
    PRIVATE
        # where the library itself will look for its internal headers
        ${CMAKE_CURRENT_SOURCE_DIR}
    PUBLIC
        # where top-level project will look for the library's public headers
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/external>
        # where external projects will look for the library's public headers
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

install(TARGETS ${PROJECT_NAME}
    DESTINATION ${INSTALL_PLUGIN_DIR}
)
