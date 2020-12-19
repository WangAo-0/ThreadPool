cmake_minimum_required(VERSION 3.12)
project(ThreadPool)

include(GNUInstallDirs)

set(CMAKE_INSTALL_CMAKEDIR "share/cmake" CACHE STRING "Install location for cmake related files")
mark_as_advanced(CMAKE_INSTALL_CMAKEDIR)

set(LIBRARY_TYPES "OBJECT;STATIC;SHARED")
set(THREADPOOL_LIBRARY_TYPE "OBJECT" CACHE STRING "Choose the type of library to build, options are: ${LIBRARY_TYPES}.")
set_property(CACHE THREADPOOL_LIBRARY_TYPE PROPERTY STRINGS ${LIBRARY_TYPES})
if (NOT THREADPOOL_LIBRARY_TYPE IN_LIST LIBRARY_TYPES)
  message(FATAL_ERROR "Invalid option: THREADPOOL_LIBRARY_TYPE=${THREADPOOL_LIBRARY_TYPE}. Supported options: ${LIBRARY_TYPES}.")
endif()

find_package(Threads REQUIRED)

add_library(ThreadPool ${THREADPOOL_LIBRARY_TYPE} ThreadPool.cpp)
target_link_libraries(ThreadPool PUBLIC Threads::Threads)

target_include_directories(ThreadPool
  PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
         $<INSTALL_INTERFACE:include>)

target_compile_features(ThreadPool PRIVATE cxx_std_17)

# Define public headers to get them automatically installed.
set_target_properties(ThreadPool PROPERTIES
  PUBLIC_HEADER "ThreadPool.hpp")

set(THREADPOOL_EXPORT ThreadPoolConfig)

# Install the lib and its headers. Flag it for export.
install(
  TARGETS ThreadPool
  EXPORT ${THREADPOOL_EXPORT}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

# Create the export file for the build tree.
export(TARGETS ThreadPool FILE "${PROJECT_BINARY_DIR}/${THREADPOOL_EXPORT}.cmake")

# Create the export file for the install tree.
install(EXPORT ${THREADPOOL_EXPORT}
  DESTINATION "${CMAKE_INSTALL_CMAKEDIR}")
