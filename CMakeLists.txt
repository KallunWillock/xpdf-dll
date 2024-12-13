#========================================================================
#
# cmake-xpdf.txt
#
# CMake script for the Xpdf package.
#
# Copyright 2015 Glyph & Cog, LLC
#
#========================================================================

cmake_minimum_required(VERSION 3.4.8)

set(CMAKE_GENERATOR_PLATFORM x64 CACHE INTERNAL "Force 32-bit compilation")

project(xpdf)

set(PNG_LIBRARY "${CMAKE_CURRENT_SOURCE_DIR}/LibPng")
set(PNG_INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/LibPng/include")
set(PNG_PNG_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/LibPng/include")

set(ZLIB_LIBRARY "${CMAKE_CURRENT_SOURCE_DIR}/ZLib")
set(ZLIB_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/ZLib/include")

set(FREETYPE_LIBRARY "${CMAKE_CURRENT_SOURCE_DIR}/FreeType")
set(FREETYPE_INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/FreeType/include")

include(cmake-config.txt)

#isaque alterou aqui
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")

message(WARNING "CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS}")   

add_subdirectory(goo)
add_subdirectory(fofi)
add_subdirectory(splash)
add_subdirectory(xpdf)
add_subdirectory(xpdf-qt)

add_subdirectory(FreeType)
add_subdirectory(LibPng)
add_subdirectory(ZLib)


if (NOT Freetype_FOUND)
  message(WARNING "Couldn't find FreeType -- will not build pdftoppm, pdftopng, pdftohtml, or xpdf.")
endif ()

if (NOT PNG_FOUND)
  message(WARNING "Couldn't find libpng -- will not build pdftopng or pdftohtml.")
endif ()

if (NOT QT4_FOUND AND NOT Qt5Widgets_FOUND)
  message(WARNING "Couldn't find Qt4 or Qt5 -- will not build xpdf.")
endif ()

if (NOT MULTITHREADED)
  message(WARNING "Multithreading is disabled -- will not build xpdf.")
endif ()

if (NOT USE_EXCEPTIONS)
  message(WARNING "C++ exceptions are disabled -- will not build xpdf.")
endif ()
