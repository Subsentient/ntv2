cmake_minimum_required(VERSION 3.10)

project(ntv2)

# CMake includes
set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake/")
include(AJAHelpers)
include(BuildOptions)
include(CommonDefines)
include(CommonFlags)
include(CommonPaths)
include(CommonVars)

# Override the following variables to set the NTV2 SDK version defines and strings used for the SDK and apps
if (AJA_BUILDING_CMAKE)
    string(TIMESTAMP AJA_BUILD_YEAR "%Y")
    string(TIMESTAMP DATETIME_NOW "\"%A %b %d, %Y (%m/%d/%Y @ %H:%M:%S GMT+8)\"")
    string(TIMESTAMP DATETIME_NOW "\"%m/%d/%Y +8:%H:%M:%S\"")
    if (NOT AJA_NTV2_SDK_VERSION_MAJOR)
        set(AJA_NTV2_SDK_VERSION_MAJOR 16)
    endif()
    if (NOT AJA_NTV2_SDK_VERSION_MINOR)
        set(AJA_NTV2_SDK_VERSION_MINOR 2)
    endif()
    if (NOT AJA_NTV2_SDK_VERSION_POINT)
        set(AJA_NTV2_SDK_VERSION_POINT 0)
    endif()
    if (NOT AJA_NTV2_SDK_BUILD_NUMBER)
        set(AJA_NTV2_SDK_BUILD_NUMBER 1)
    endif()
    set(AJA_NTV2_SDK_BUILD_DATETIME ${DATETIME_NOW})
    set(AJA_NTV2_SDK_BUILD_TYPE "\"d\"") # r = release, d = debug, b = beta
    set(AJA_NTV2_VER_STR "${AJA_NTV2_SDK_VERSION_MAJOR}.${AJA_NTV2_SDK_VERSION_MINOR}.${AJA_NTV2_SDK_VERSION_POINT}")
    set(AJA_NTV2_VER_STR_LONG "${AJA_NTV2_VER_STR}.${AJA_NTV2_SDK_BUILD_NUMBER}")
    string(REPLACE "." "," AJA_NTV2_VER_STR_COMMA "${AJA_NTV2_VER_STR_LONG}")

    string(REPLACE "\"" "" aja_ntv2_build_type_letter "${AJA_NTV2_SDK_BUILD_TYPE}")
    message(STATUS "ntv2 version: ${AJA_NTV2_VER_STR} ${aja_ntv2_build_type_letter}${AJA_NTV2_SDK_BUILD_NUMBER}")
endif()

message(STATUS "Platform: ${CMAKE_SYSTEM_NAME}")
message(STATUS "Arch: ${CMAKE_HOST_SYSTEM_PROCESSOR}")
message(STATUS "Compiler: ${CMAKE_CXX_COMPILER_ID}")
if (CMAKE_SYSTEM_NAME STREQUAL "Windows")
    message(STATUS "Windows API Version: ${CMAKE_SYSTEM_VERSION}")
    string(REPLACE "." ";" AJA_WIN_API_VER "${CMAKE_SYSTEM_VERSION}")
    list(GET AJA_WIN_API_VER 0 AJA_WIN_API_VER_MAJOR)
    list(GET AJA_WIN_API_VER 1 AJA_WIN_API_VER_MINOR)
    list(GET AJA_WIN_API_VER 2 AJA_WIN_API_VER_BUILD)
elseif(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    message (STATUS "macOS Architectures: ${CMAKE_OSX_ARCHITECTURES}")
    message (STATUS "macOS Deployment Target: ${CMAKE_OSX_DEPLOYMENT_TARGET}")
elseif(CMAKE_SYSTEM_NAME STREQUAL "Linux")
else()
    message(FATAL_ERROR "Unsupported platform: ${CMAKE_SYSTEM_NAME}")
endif()

message(STATUS "cmake install destinations (when running cmake --install):\n\
    AJA_INSTALL_PREFIX = ${AJA_INSTALL_PREFIX}\n\
    AJA_INSTALL_INCLUDEDIR = ${AJA_INSTALL_INCLUDEDIR}\n\
    AJA_INSTALL_LIBDIR = ${AJA_INSTALL_LIBDIR}\n\
    AJA_INSTALL_BINDIR = ${AJA_INSTALL_BINDIR}")

# Optional build artifacts output directory
if (AJA_DEPLOY_OUTPUTS)
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY
        "${AJA_CMAKE_DIR}/../out/${CMAKE_SYSTEM_NAME}-${CMAKE_CXX_COMPILER_ID}-${AJA_BITS}bit/lib")
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY
        "${AJA_CMAKE_DIR}/../out/${CMAKE_SYSTEM_NAME}-${CMAKE_CXX_COMPILER_ID}-${AJA_BITS}bit/lib")
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY
        "${AJA_CMAKE_DIR}/../out/${CMAKE_SYSTEM_NAME}-${CMAKE_CXX_COMPILER_ID}-${AJA_BITS}bit/bin")
endif()

# Add NTV2 sub-directories to CMake build config
if (AJA_BUILD_APPS AND EXISTS ${AJA_APPS_ROOT})
    message(STATUS "Adding ajaapps...")
    add_subdirectory(ajaapps)
endif()
if (AJA_BUILD_DRIVER AND EXISTS ${AJA_DRIVER_ROOT})
    message(STATUS "Adding ajadriver...")
    add_subdirectory(ajadriver)
endif()
if (AJA_BUILD_LIBS AND EXISTS ${AJA_LIBRARIES_ROOT})
    message(STATUS "Adding ajalibraries...")
    add_subdirectory(ajalibraries)
endif()
if (AJA_BUILD_PLUGINS AND EXISTS ${AJA_PLUGINS_ROOT})
    message(STATUS "Adding ajaplugins...")
    add_subdirectory(ajaplugins)
endif()
if (AJA_BUILD_QA AND EXISTS ${AJA_QA_ROOT})
    message(STATUS "Adding qa...")
    add_subdirectory(qa)
endif()
if (AJA_BUILD_NONAJA AND EXISTS ${NON_AJA_ROOT})
	message(STATUS "Adding non-aja...")
	add_subdirectory(non-aja)
endif()
