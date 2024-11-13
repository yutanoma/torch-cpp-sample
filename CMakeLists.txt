cmake_minimum_required(VERSION 3.13)
project(MyProject)

include(FetchContent)

# Fetch libtorch
if(APPLE)
  set(LIBTORCH_URL "https://download.pytorch.org/libtorch/cpu/libtorch-macos-arm64-2.5.1.zip")
elseif(UNIX)
  set(LIBTORCH_URL "https://download.pytorch.org/libtorch/cpu/libtorch-shared-with-deps-2.5.1%2Bcpu.zip")
elseif(WIN32)
  set(LIBTORCH_URL "https://download.pytorch.org/libtorch/cpu/libtorch-win-shared-with-deps-2.5.1%2Bcpu.zip")
endif()

FetchContent_Declare(
    libtorch
    URL ${LIBTORCH_URL}
    DOWNLOAD_EXTRACT_TIMESTAMP TRUE
)
FetchContent_MakeAvailable(libtorch)

# Set CMAKE_PREFIX_PATH to the libtorch directory so find_package can locate Torch
set(CMAKE_PREFIX_PATH ${libtorch_SOURCE_DIR})

# Find the Torch package
find_package(Torch REQUIRED)

# Your source files
set(SRCS
  src/main.cpp
)

# Add the executable target
add_executable(main ${SRCS})

# Link the Torch libraries
target_link_libraries(main "${TORCH_LIBRARIES}")

# Set the C++ standard (if needed)
set_property(TARGET main PROPERTY CXX_STANDARD 20)
