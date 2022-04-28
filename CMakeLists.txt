cmake_minimum_required(VERSION 3.0)
project(octo_sync)

set(CMAKE_CXX_STANDARD 17)

add_executable(octo_sync ${CMAKE_SOURCE_DIR}/src/octo_sync.cpp)

include(ExternalProject)

set_target_properties(octo_sync PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/bin")
set_directory_properties(PROPERTIES EP_PREFIX ${CMAKE_SOURCE_DIR}/ext/rust-log-parser)

ExternalProject_Add(
    rust-log-parser
    DOWNLOAD_COMMAND ""
    CONFIGURE_COMMAND ""
    BUILD_COMMAND cargo build --release
    BINARY_DIR "${CMAKE_SOURCE_DIR}/ext/rust-log-parser"
    INSTALL_COMMAND cp ${CMAKE_SOURCE_DIR}/ext/rust-log-parser/target/release/log_parser ${CMAKE_SOURCE_DIR}/bin/log_parser
)

add_dependencies(octo_sync rust-log-parser)