cmake_minimum_required(VERSION 3.10)

project(libdeflate C)

# --- Options ---
option(LIBDEFLATE_BUILD_SHARED "Build libdeflate as a shared library" ${BUILD_SHARED_LIBS})
option(LIBDEFLATE_BUILD_STATIC "Build libdeflate as a static library" ON)

# --- Sources ---
set(LIBDEFLATE_SOURCES
    lib/adler32.c
    lib/crc32.c
    lib/deflate_compress.c
    lib/deflate_decompress.c
    lib/gzip_compress.c
    lib/gzip_decompress.c
    lib/zlib_compress.c
    lib/zlib_decompress.c
    lib/utils.c
)

# Platform specific feature detection sources
if (CMAKE_SYSTEM_PROCESSOR MATCHES "x86_64" OR CMAKE_SYSTEM_PROCESSOR MATCHES "AMD64" OR CMAKE_SYSTEM_PROCESSOR MATCHES "i386")
    list(APPEND LIBDEFLATE_SOURCES lib/x86/x86_cpu_features.c)
elseif (CMAKE_SYSTEM_PROCESSOR MATCHES "arm" OR CMAKE_SYSTEM_PROCESSOR MATCHES "aarch64")
    list(APPEND LIBDEFLATE_SOURCES lib/arm/arm_cpu_features.c)
endif()

# --- Shared Library Configuration ---
if (LIBDEFLATE_BUILD_SHARED)
    add_library(libdeflate_shared SHARED ${LIBDEFLATE_SOURCES})
    set_target_properties(libdeflate_shared PROPERTIES 
        OUTPUT_NAME deflate
        C_VISIBILITY_PRESET hidden
    )
    target_compile_definitions(libdeflate_shared PUBLIC LIBDEFLATE_DLL)
    target_include_directories(libdeflate_shared
        PUBLIC
            $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
            $<INSTALL_INTERFACE:include>
        PRIVATE
            ${CMAKE_CURRENT_SOURCE_DIR}/lib
    )
    add_library(libdeflate::libdeflate_shared ALIAS libdeflate_shared)
    
    if (NOT TARGET libdeflate)
        add_library(libdeflate ALIAS libdeflate_shared)
        add_library(libdeflate::libdeflate ALIAS libdeflate_shared)
    endif()
endif()

# --- Static Library Configuration ---
if (LIBDEFLATE_BUILD_STATIC)
    add_library(libdeflate_static STATIC ${LIBDEFLATE_SOURCES})
    set_target_properties(libdeflate_static PROPERTIES 
        OUTPUT_NAME deflate
        POSITION_INDEPENDENT_CODE ON
    )
    target_include_directories(libdeflate_static
        PUBLIC
            $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
            $<INSTALL_INTERFACE:include>
        PRIVATE
            ${CMAKE_CURRENT_SOURCE_DIR}/lib
    )
    add_library(libdeflate::libdeflate_static ALIAS libdeflate_static)
    
    if (NOT TARGET libdeflate)
        add_library(libdeflate ALIAS libdeflate_static)
        add_library(libdeflate::libdeflate ALIAS libdeflate_static)
    endif()
endif()

# --- Installation ---
set(INSTALL_TARGETS)
if (TARGET libdeflate_static)
    list(APPEND INSTALL_TARGETS libdeflate_static)
endif()
if (TARGET libdeflate_shared)
    list(APPEND INSTALL_TARGETS libdeflate_shared)
endif()

if (INSTALL_TARGETS)
    install(TARGETS ${INSTALL_TARGETS}
        EXPORT libdeflateTargets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin
        INCLUDES DESTINATION include
    )

    install(FILES libdeflate.h DESTINATION include)

    install(EXPORT libdeflateTargets
        FILE libdeflateConfig.cmake
        NAMESPACE libdeflate::
        DESTINATION lib/cmake/libdeflate
    )
endif()
