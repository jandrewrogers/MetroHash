cmake_minimum_required(VERSION 3.23 FATAL_ERROR)

project(MetroHash
    VERSION 1.1.3
    DESCRIPTION "MetroHash: Faster, Better Hash Functions"
    HOMEPAGE_URL "http://www.jandrewrogers.com/2015/05/27/metrohash/"
    LANGUAGES CXX
)

list(APPEND CMAKE_MESSAGE_CONTEXT top)

if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR AND NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
    set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

set(METRO_HASH_INSTALL_CMAKEDIR cmake CACHE PATH "Installation directory for config-file package cmake files")

add_library(metrohash)
target_sources(metrohash
    PRIVATE
        src/metrohash64.cpp
        src/metrohash128.cpp
        src/metrohash128crc.cpp
        src/metrohash.h
        src/metrohash64.h
        src/metrohash128.h
        src/metrohash128crc.h
        src/platform.h
        src/testvector.h
)
target_include_directories(metrohash
    PRIVATE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
)
target_compile_features(metrohash PUBLIC cxx_std_11)
target_compile_options(metrohash
    PRIVATE
        $<$<CXX_COMPILER_ID:MSVC>:/W4 /WX> 
        $<$<CXX_COMPILER_ID:GNU,Clang,AppleClang>:-Wall -Werror>
)

set_target_properties(metrohash
    PROPERTIES
		VERSION ${PROJECT_VERSION} # Contains 1.2.3
		SOVERSION ${PROJECT_VERSION_MAJOR} # Contains only 1
)
set_target_properties(metrohash
    PROPERTIES
        DEBUG_POSTFIX d
)

include(GNUInstallDirs)

install(TARGETS metrohash
    EXPORT
        metrohash_export
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

install (
    FILES
        ${PROJECT_SOURCE_DIR}/src/metrohash.h
        ${PROJECT_SOURCE_DIR}/src/metrohash64.h
        ${PROJECT_SOURCE_DIR}/src/metrohash128.h
        ${PROJECT_SOURCE_DIR}/src/metrohash128crc.h
    DESTINATION
        ${CMAKE_INSTALL_INCLUDEDIR}
)

install(EXPORT metrohash_export
    FILE
        metrohash-config.cmake
    NAMESPACE
        MetroHash::
    DESTINATION
        ${METRO_HASH_INSTALL_CMAKEDIR}
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "metrohash-config-version.cmake"
    COMPATIBILITY SameMajorVersion
)

install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/metrohash-config-version.cmake"
    DESTINATION "${METRO_HASH_INSTALL_CMAKEDIR}"
)