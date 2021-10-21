cmake_minimum_required(VERSION 3.8)
project(gzip LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED on)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# set some config CMAKE_INSTALL_PREFIX
# set(CMAKE_MSVC_RUNTIME_LIBRARY MultiThreadedDLL)
# set(CMAKE_INSTALL_PREFIX ${CMAKE_CURRENT_LIST_DIR}/ThirdParty)
# set(CMAKE_INSTALL_LIBDIR ${CMAKE_INSTALL_PREFIX}/lib)
# configure optimization

if (CMAKE_BUILD_TYPE STREQUAL "Debug")
    set(OPTIMIZATION_FLAGS "-O0 -DDEBUG")
    message("-- Configuring debug build")
else()
    set(OPTIMIZATION_FLAGS "-O3 -DNDEBUG")
    message("-- Configuring release build")
endif()

# Note: -D_GLIBCXX_USE_CXX11_ABI=0 is needed to support mason packages that are precompiled libs
# Currently we only depend on a header only library, but this will help avoid issues when more libs are added via mason
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OPTIMIZATION_FLAGS} -D_GLIBCXX_USE_CXX11_ABI=0 ${DESIRED_WARNINGS}")

if (WERROR)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Werror")
endif()

install(DIRECTORY "${PROJECT_SOURCE_DIR}/include/gzip" TYPE INCLUDE)
add_subdirectory(extern)
install(TARGETS zlib)
install(FILES ${ZLIB_INCLUDE_HDRS} TYPE INCLUDE)


