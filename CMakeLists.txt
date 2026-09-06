cmake_minimum_required(VERSION 3.27)
project(RedFileSystem VERSION 0.15.1)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED YES)

configure_file(src/Config.h.in Config/Config.h)

set(INC_FILES
        src/AsyncFile.h
        src/File.h
        src/FilePromise.h
        src/FileSystem.h
        src/FileSystemStorage.h

        src/FileSystemStatus.h
        src/FileSystemWriteMode.h

        src/Utils.h
        src/Base64.h
)

set(SRC_FILES
        src/main.cpp

        src/AsyncFile.cpp
        src/File.cpp
        src/FileSystem.cpp
        src/FileSystemStorage.cpp
)

source_group(include FILES INC_FILES)
source_group(source FILES SRC_FILES)

add_library(RedFileSystem SHARED
        ${INC_FILES}
        ${SRC_FILES}
)

target_include_directories(RedFileSystem PRIVATE src/)

# Include configured files
target_include_directories(RedFileSystem PUBLIC "${PROJECT_BINARY_DIR}/Config")

add_compile_definitions(
        # Exclude unused Windows headers
        WIN32_LEAN_AND_MEAN

        # Use Unicode charset.
        UNICODE
        _UNICODE
)

# RedData provides the same RED4ext.SDK and RedLib revisions as this project.
# Let it create the shared CMake targets once to avoid duplicate target errors.
option(RED4EXT_HEADER_ONLY "" ON)
add_compile_definitions(NOMINMAX)
add_subdirectory(deps/RedData)
set_target_properties(RED4ext.SDK RedLib RedData PROPERTIES FOLDER "Dependencies")
mark_as_advanced(RED4EXT_BUILD_EXAMPLES RED4EXT_HEADER_ONLY)

target_link_libraries(RedFileSystem PRIVATE
        RED4ext::SDK
        RedLib
        RedData
)

## Debug mode: install scripts (+ tests) and plugin in game folder.
## Release mode: create archive with bundled scripts and plugin.
add_custom_command(
        TARGET RedFileSystem
        POST_BUILD
        WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
        COMMENT "$<IF:$<CONFIG:Debug>,Install scripts with red-cli,Build archive with red-cli>"
        COMMAND "$<$<CONFIG:Debug>:red-cli;install>" "$<$<CONFIG:Release>:red-cli;pack>"
        COMMAND_EXPAND_LISTS
)
