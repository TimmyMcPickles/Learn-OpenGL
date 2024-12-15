cmake_minimum_required(VERSION 3.16)
project(app VERSION 0.1.0 DESCRIPTION "My game")

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_BINARY_DIR ${CMAKE_SOURCE_DIR}/bin)
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR})

# Option to use system-installed dependencies or fetch them
option(USE_SYSTEM_DEPENDENCIES "Use system-installed dependencies" OFF)

# Handle dependencies
include(FetchContent)

# GLFW
if(USE_SYSTEM_DEPENDENCIES)
    find_package(glfw3 3.3 REQUIRED)
else()
    FetchContent_Declare(
        glfw
        GIT_REPOSITORY https://github.com/glfw/glfw.git
        GIT_TAG 3.3.8
    )
    set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
    set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
    set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
    FetchContent_MakeAvailable(glfw)
endif()

# OpenGL
find_package(OpenGL REQUIRED)

# Source files
file(GLOB_RECURSE SOURCE_FILES
    ${CMAKE_SOURCE_DIR}/src/*.cpp
    ${CMAKE_SOURCE_DIR}/src/*.c
)

file(GLOB_RECURSE HEADER_FILES
    ${CMAKE_SOURCE_DIR}/include/*.hpp
    ${CMAKE_SOURCE_DIR}/include/*.h
)

# Create executable
add_executable(${PROJECT_NAME} ${SOURCE_FILES} ${HEADER_FILES})

# Include directories
target_include_directories(${PROJECT_NAME}
    PRIVATE
        ${CMAKE_SOURCE_DIR}/include
)

# Platform-specific settings
if(WIN32)
    target_link_libraries(${PROJECT_NAME}
        PRIVATE
            glfw
            OpenGL::GL
    )
elseif(UNIX AND NOT APPLE)
    target_link_libraries(${PROJECT_NAME}
        PRIVATE
            glfw
            OpenGL::GL
            X11
            pthread
            dl
    )
elseif(APPLE)
    target_link_libraries(${PROJECT_NAME}
        PRIVATE
            glfw
            OpenGL::GL
            "-framework Cocoa"
            "-framework IOKit"
            "-framework CoreVideo"
    )
endif()

# Installation rules
install(TARGETS ${PROJECT_NAME}
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)
