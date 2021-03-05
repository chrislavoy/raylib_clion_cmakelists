cmake_minimum_required(VERSION 3.17)
project(${PROJECT_NAME} C)

set(CMAKE_C_STANDARD 99)

# The rest of the file came from
# https://www.reddit.com/r/raylib/comments/ecz6ja/problem_getting_started_with_project_cmake_file/fbkip7u?utm_source=share&utm_medium=web2x&context=3

# Set this to the minimal version you want to support
find_package(raylib 3.5 QUIET) # Let CMake search for a raylib-config.cmake

# You could change the QUIET above to REQUIRED and remove this if() clause
# This part downloads raylib and builds it if it's not installed on your system
if (NOT raylib_FOUND) # If there's none, fetch and build raylib
    include(FetchContent)

    FetchContent_Declare(
            raylib
            URL https://github.com/raysan5/raylib/archive/master.tar.gz
    )

    FetchContent_GetProperties(raylib)
    if (NOT raylib_POPULATED) # Have we downloaded raylib yet?
        set(FETCHCONTENT_QUIET NO)
        FetchContent_Populate(raylib)

        set(BUILD_EXAMPLES OFF CACHE BOOL "" FORCE) # don't build the supplied examples
        set(BUILD_GAMES    OFF CACHE BOOL "" FORCE) # or games

        # build raylib
        add_subdirectory(${raylib_SOURCE_DIR} ${raylib_BINARY_DIR})

    endif()

endif()

# This is the main part:

add_executable(${PROJECT_NAME} main.c)
#set(raylib_VERBOSE 1)
target_link_libraries(${PROJECT_NAME} raylib)

# That's it! You should have an example executable that you can run. Have fun!
