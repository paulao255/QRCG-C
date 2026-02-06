# Project configuration:
cmake_minimum_required(VERSION 4.2.3)
project(QRCG LANGUAGES C VERSION 20260206)
include(FetchContent)

# Version variables:
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS ON)

# Other variables:
set(CMAKE_POLICY_VERSION_MINIMUM 3.5)
set(WITH_PNG OFF CACHE BOOL "" FORCE)
set(WITH_TOOLS OFF CACHE BOOL "" FORCE)

# Declare dependencies:
FetchContent_Declare(C-Utils GIT_REPOSITORY https://github.com/paulao255/C-Utils.git GIT_TAG main)
FetchContent_Declare(qrencode GIT_REPOSITORY https://github.com/fukuchi/libqrencode.git GIT_TAG master)
FetchContent_Declare(stb GIT_REPOSITORY https://github.com/nothings/stb.git GIT_TAG master)

# Build dependencies:
FetchContent_MakeAvailable(C-Utils)
FetchContent_MakeAvailable(qrencode)
FetchContent_MakeAvailable(stb)

# Build the executable:
add_executable(QRCode-Generator ../src/Main.c)
target_compile_options(QRCode-Generator PUBLIC -fdiagnostics-color=always -Wall -Wextra -Wpedantic -Wshadow -Wconversion -Wsign-conversion -Wformat=2 -Wnull-dereference -Wstrict-prototypes -Wmissing-prototypes -Wcast-align -Wpointer-arith -Wundef -v -O3 -funroll-loops -flto -mtune=native -march=native)

# Link dependencies:
target_include_directories(QRCode-Generator PUBLIC ${stb_SOURCE_DIR})
target_link_libraries(QRCode-Generator PRIVATE C-Utils qrencode)

# Set output directory:
set_target_properties(QRCode-Generator PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
