cmake_minimum_required(VERSION 3.14)
project(plink2)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(ZSTD_BUILD_SHARED OFF CACHE BOOL "" FORCE)

add_subdirectory(zstd)
add_subdirectory(libdeflate)
add_subdirectory(src)

# Create main exe
add_executable(plink2 src/plink2.cc src/SFMT.c)

# Link libraries
find_package(BLAS REQUIRED)
find_package(Threads REQUIRED)
find_package(ZLIB REQUIRED)

target_link_libraries(plink2 
    plink2_lib 
    pgen_lib  
    zstd 
    libdeflate::libdeflate 
    ${BLAS_LIBRARIES} 
    Threads::Threads
    ZLIB::ZLIB
)

# Link supplement headers
target_include_directories(plink2 PUBLIC ${CMAKE_SOURCE_DIR}/include)