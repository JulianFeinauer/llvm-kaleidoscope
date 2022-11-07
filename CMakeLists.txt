cmake_minimum_required(VERSION 3.4.3)
project(Kaleiscope)

set(CMAKE_CXX_STANDARD 17)

#set(ENV{LLVM_DIR} /Users/julian/Develop/llvm-project/build/cmake/modules/CMakeFiles/)
#include(/Users/julian/Develop/llvm-project/build/cmake/modules/CMakeFiles/LLVMConfig.cmake)

find_package(LLVM REQUIRED CONFIG PATHS /Users/julian/Develop/llvm-project/build/ NO_DEFAULT_PATH)
find_package(CLANG REQUIRED CONFIG PATHS /Users/julian/Develop/llvm-project/build/ NO_DEFAULT_PATH)

#################################################################
# Set gdb
#################################################################
SET(CMAKE_BUILD_TYPE "Debug")
SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g2 -ggdb")
SET(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")

message(STATUS "Found LLVM ${LLVM_PACKAGE_VERSION}")
message(STATUS "Using LLVMConfig.cmake in: ${LLVM_DIR}")
# Set your project compile flags.
# E.g. if using the C++ header files
# you will need to enable C++11 support
# for your compiler.

#include_directories("libs")
add_definitions(${LLVM_DEFINITIONS})
add_definitions(${CLANG_DEFINITIONS})

include_directories(${LLVM_INCLUDE_DIRS})
include_directories(${CLANG_INCLUDE_DIRS})

#################################################################
# Now build our tools
#################################################################
add_executable(toy toy.cpp KaleidoscopeJIT.h)

#################################################################
# Set Flags
#################################################################
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -fno-rtti")

#################################################################
# Find the libraries that correspond to the LLVM components
# that we wish to use
#################################################################

set(LLVM_LINK_COMPONENTS
        Core
        ExecutionEngine
        Interpreter
        MC
        Support
        nativecodegen
        mcjit
        asmparser
        objcarcopts
        scalaropts
        transformutils
        irreader
        aarch64asmparser # aarch64codegen aarch64desc aarch64disassembler aarch64info aarch64utils
        amdgpuasmparser amdgpucodegen
        armasmparser armcodegen
        avrasmparser avrcodegen
        bpfasmparser bpfcodegen
        hexagonasmparser hexagoncodegen
        lanaiasmparser lanaicodegen
        msp430asmparser msp430codegen
        mipsasmparser mipscodegen
        nvptxcodegen
        powerpcasmparser powerpccodegen
        riscvasmparser riscvcodegen
        sparcasmparser sparccodegen
        systemzasmparser systemzcodegen
        veasmparser vecodegen
        webassemblyasmparser webassemblycodegen
        x86asmparser x86codegen
        xcorecodegen
        )

llvm_map_components_to_libnames(llvm_libs ${LLVM_LINK_COMPONENTS})

## See https://stackoverflow.com/a/62939671
#execute_process(COMMAND ${LLVM_DIR}/../../../bin/llvm-config --libs all
#        OUTPUT_VARIABLE llvm_libs OUTPUT_STRIP_TRAILING_WHITESPACE)

message("LLVM Libs: ${llvm_libs}")


#################################################################
# Add Dynamic .so library
#################################################################
#link_directories(./bin) #添加动态连接库的路径
#add_library( libhello SHARED IMPORTED )
target_link_libraries(toy ${llvm_libs})

#################################################################
# Add Static .a library
#################################################################
#add_library(mxnet STATIC IMPORTED)
#set_property(TARGET mxnet PROPERTY IMPORTED_LOCATION /path/to/libmxnet.a)
#target_link_libraries(project_name mxnet ) #添加libmxnet.a

