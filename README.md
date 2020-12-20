# ThreadPool [![pipeline status](https://gitlab.com/jhasse/ThreadPool/badges/master/pipeline.svg)](https://gitlab.com/jhasse/ThreadPool/commits/master)

A simple C++17 Thread Pool implementation.

## Basic usage

```c++
// create thread pool with 4 worker threads
ThreadPool pool(4);

// enqueue and store future
auto result = pool.enqueue([](int answer) { return answer; }, 42);

// get result from future
std::cout << result.get() << std::endl;

```

## CMake

### Build options

- `THREADPOOL_LIBRARY_TYPE`: Set the type of library to build.

    Supported options: `OBJECT` (default), `STATIC`, `SHARED`.

### Import using `find_package`

Build and install ThreadPool somewhere on your system (note: `OBJECT` libraries cannot be installed).

```cmake
find_package(Threads REQUIRED)

set(ThreadPool_DIR "</threadpool/install/dir>/share/cmake")
# You can actually skip the install step and use the build directory.
set(ThreadPool_DIR "</threadpool/build/dir>")

find_package(Threads REQUIRED)
find_package(ThreadPool)

target_link_libraries(<your-target> PRIVATE ThreadPool)
```

### Import using `FetchContent` (CMake >= 3.11)

```cmake
find_package(Threads REQUIRED)

FetchContent_Declare(ThreadPool
    GIT_REPOSITORY git://github.com/jhasse/ThreadPool.git
)

# Optional
# set(THREADPOOL_LIBRARY_TYPE SHARED)

# If CMake >= 3.14
FetchContent_MakeAvailable(ThreadPool)

# If CMake < 3.14
FetchContent_GetProperties(ThreadPool)
if (NOT ThreadPool_POPULATED)
    FetchContent_Populate(ThreadPool)
    add_subdirectory("${ThreadPool_SOURCE_DIR}" ${ThreadPool_BINARY_DIR})
endif()

target_link_libraries(<your-target> PRIVATE ThreadPool)
```