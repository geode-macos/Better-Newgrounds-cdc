cmake_minimum_required(VERSION 3.21)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
if ("${CMAKE_SYSTEM_NAME}" STREQUAL "iOS" OR IOS)
	set(CMAKE_OSX_ARCHITECTURES "arm64")
else()
	set(CMAKE_OSX_ARCHITECTURES "arm64;x86_64")
endif()
set(CMAKE_CXX_VISIBILITY_PRESET hidden)

project(BetterNewgrounds VERSION 1.0.0)

add_library(${PROJECT_NAME} SHARED
    src/main.cpp
    src/NewgroundsBrowserLayer.cpp
    src/NewgroundsAPI.cpp
    src/NewgroundsSongCell.cpp
    src/Clip.cpp
    src/AudioWaveformNode.cpp
    src/NewgroundsGenreLabel.cpp
    src/Popups/NewgroundsArtistPopup.cpp
    src/Popups/NewgroundsSongPopup.cpp
    src/tinyxml2.cpp
    # Add any extra C++ source files here
)

if (NOT DEFINED ENV{GEODE_SDK})
    message(FATAL_ERROR "Unable to find Geode SDK! Please define GEODE_SDK environment variable to point to Geode")
else()
    message(STATUS "Found Geode: $ENV{GEODE_SDK}")
endif()

add_subdirectory($ENV{GEODE_SDK} ${CMAKE_CURRENT_BINARY_DIR}/geode)
add_compile_definitions(TINYXML2_INCLUDED)
setup_geode_mod(${PROJECT_NAME})
