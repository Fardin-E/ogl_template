cmake_minimum_required(VERSION 3.10)

set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Release>:Release>")

# add the library inside thirdparty and add as a subdirectory

project(model_viewer)

set(CMAKE_CXX_STANDARD 17)

# define SOURCES as all source files for the project
file(GLOB_RECURSE SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)
list(APPEND SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/src/glad.c) # Add glad.c explicitly
add_executable(${PROJECT_NAME} ${SOURCES})

# textures can be found in the TEXTURES_PATH macro
target_compile_definitions(${CMAKE_PROJECT_NAME} PUBLIC TEXTURES_PATH="${CMAKE_CURRENT_SOURCE_DIR}/textures/")
# target_compile_definitions(${CMAKE_PROJECT_NAME} PUBLIC TEXTURES_PATH="./textures/")

# shaders can be found in the SHADERS_PATH macro
target_compile_definitions(${CMAKE_PROJECT_NAME} PUBLIC SHADERS_PATH="${CMAKE_CURRENT_SOURCE_DIR}/shaders/")
# target_compile_definitions(${CMAKE_PROJECT_NAME} PUBLIC SHADERS_PATH="./shaders/")

target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include/)

# then link the libraries with the project
# Link against the correct GLFW library based on the build type
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    target_link_libraries(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/lib/Debug/glfw3.lib)
elseif(CMAKE_BUILD_TYPE STREQUAL "Release")
    target_link_libraries(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/lib/Release/glfw3.lib)
else()
    message(WARNING "Build type is not Debug or Release. Linking against Debug library by default.")
    target_link_libraries(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/lib/Debug/glfw3.lib)
endif()