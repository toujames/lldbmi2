cmake_minimum_required(VERSION 3.1)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(lldbmi2)

execute_process(COMMAND git log --oneline
                COMMAND wc -l
                COMMAND tr -d " "
                COMMAND tr -d "\n"
                WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
                OUTPUT_VARIABLE version_revision)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/templates/version.h.in ${CMAKE_CURRENT_BINARY_DIR}/generated/version.h)

find_library(LLDB_LIBRARY LLDB PATHS "/Applications/Xcode.app/Contents/SharedFrameworks")
mark_as_advanced(LLDB_LIBRARY)

set(CMAKE_INSTALL_RPATH "/Applications/Xcode.app/Contents/SharedFrameworks")

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/lldb-headers)
include_directories(${CMAKE_CURRENT_BINARY_DIR}/generated)

file(GLOB_RECURSE lldbmi2_sources ${CMAKE_CURRENT_SOURCE_DIR}/lldbmi2/Sources/*.cpp)

add_executable(${PROJECT_NAME} ${lldbmi2_sources})
target_link_libraries(${PROJECT_NAME} ${LLDB_LIBRARY})

install(TARGETS ${PROJECT_NAME} DESTINATION bin)