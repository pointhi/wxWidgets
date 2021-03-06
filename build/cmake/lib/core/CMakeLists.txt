#############################################################################
# Name:        build/cmake/lib/core/CMakeLists.txt
# Purpose:     CMake file for core library
# Author:      Tobias Taschner
# Created:     2016-10-01
# Copyright:   (c) 2016 wxWidgets development team
# Licence:     wxWindows licence
#############################################################################

include(../../source_groups.cmake)

wx_append_sources(CORE_SRC GUI_CMN)
wx_append_sources(CORE_SRC BASE_AND_GUI_CMN)
if(WIN32)
    wx_append_sources(CORE_SRC BASE_AND_GUI_WIN32)
elseif(UNIX)
    wx_append_sources(CORE_SRC UNIX)
endif()

if(WXMSW)
    wx_append_sources(CORE_SRC MSW_LOWLEVEL)
    wx_append_sources(CORE_SRC MSW_DESKTOP_LOWLEVEL)
    wx_append_sources(CORE_SRC MSW)
    wx_append_sources(CORE_SRC MSW_DESKTOP)
elseif(WXGTK)
    if(WXGTK2)
        wx_append_sources(CORE_SRC GTK2_LOWLEVEL)
        wx_append_sources(CORE_SRC GTK2)
    else()
        wx_append_sources(CORE_SRC GTK_LOWLEVEL)
        wx_append_sources(CORE_SRC GTK)
    endif()

    if(UNIX)
        wx_append_sources(CORE_SRC XWIN_LOWLEVEL)
    elseif(WIN32)
        wx_append_sources(CORE_SRC GTK_WIN32)
    endif()
elseif(WXOSX_COCOA)
    wx_append_sources(CORE_SRC BASE_AND_GUI_OSX_COCOA)
    wx_append_sources(CORE_SRC OSX_LOWLEVEL)
    wx_append_sources(CORE_SRC OSX_SHARED)
    wx_append_sources(CORE_SRC OSX_COCOA)
endif()

wx_add_library(core ${CORE_SRC})
foreach(lib JPEG PNG TIFF)
    if(${lib}_LIBRARIES)
        if(lib STREQUAL JPEG)
            wx_lib_include_directories(core PRIVATE ${${lib}_INCLUDE_DIR})
        else()
            wx_lib_include_directories(core PRIVATE ${${lib}_INCLUDE_DIRS})
        endif()

        wx_lib_link_libraries(core PRIVATE ${${lib}_LIBRARIES})
    endif()
endforeach()

if(WIN32)
    wx_lib_link_libraries(core PRIVATE
        winmm
    )
endif()
if(WXOSX_COCOA)
    wx_lib_link_libraries(core PUBLIC
        "-framework AudioToolbox"
        )
endif()

wx_finalize_lib(core)
