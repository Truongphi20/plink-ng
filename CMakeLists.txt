cmake_minimum_required(VERSION 3.14)
project(plink2)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

add_subdirectory(src)
add_subdirectory(zstd)

# Create main exe
add_executable(plink2 src/plink2.cc)

# Link libraries
target_link_libraries(plink2 plink2_lib pgen_lib others)