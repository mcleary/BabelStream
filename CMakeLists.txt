cmake_minimum_required(VERSION 3.4.3)

project(BabelStream)

set(COMPUTECPP_BITCODE "spir64")

list(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/Modules)
find_package(ComputeCpp REQUIRED)

set(CMAKE_CXX_STANDARD 14)

set(CMAKE_SKIP_BUILD_RPATH FALSE)

add_executable(sycl-stream
		main.cpp
		SYCLStream.cpp
)

add_sycl_to_target(
		TARGET sycl-stream
		SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/SYCLStream.cpp
)

set_property(TARGET sycl-stream PROPERTY COMPUTECPP_INCLUDE_AFTER 1)
target_include_directories(sycl-stream PRIVATE ${ComputeCpp_DIR}/include)
target_compile_definitions(sycl-stream PRIVATE -DSYCL)

set_property(TARGET sycl-stream PROPERTY BUILD_RPATH_USE_ORIGIN TRUE)
set_property(TARGET sycl-stream PROPERTY BUILD_RPATH "$ORIGIN/./")

add_executable(ocl-stream
		main.cpp
		OCLStream.cpp
)

set_property(TARGET ocl-stream PROPERTY BUILD_RPATH_USE_ORIGIN TRUE)
set_property(TARGET ocl-stream PROPERTY BUILD_RPATH "$ORIGIN/./")

target_compile_definitions(ocl-stream PRIVATE -DOCL)
target_link_libraries(ocl-stream PRIVATE OpenCL::OpenCL)
target_link_libraries(sycl-stream PRIVATE OpenCL::OpenCL)