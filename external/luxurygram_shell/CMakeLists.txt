# This file is part of Desktop App Toolkit,
# a set of libraries for developing nice desktop applications.
#
# For license and copyright information please follow this link:
# https://github.com/desktop-app/legal/blob/master/LEGAL

# The GPUI history bridge is a DLL the client loads at run time through
# LoadLibrary (history/view/history_view_gpui_bridge.cpp), never a link
# dependency: it carries its own Rust runtime, which must not be linked into
# Telegram beside the one inside tdesktop_rust. This target only names the
# prepared DLL through its LUXURYGRAM_SHELL_DLL property, which
# Telegram/CMakeLists.txt reads to copy it beside the executable after the
# build. Without the DLL the client is unchanged.

add_library(external_luxurygram_shell INTERFACE)
add_library(desktop-app::external_luxurygram_shell ALIAS external_luxurygram_shell)

# No init_target: it adds compile features, which an INTERFACE target cannot carry.

if (WIN32)
    set(luxurygram_shell_loc ${libs_loc}/luxurygram_shell/out)
    set(luxurygram_shell_dll "${luxurygram_shell_loc}/luxurygram_shell.dll")
    if (EXISTS "${luxurygram_shell_dll}")
        set_target_properties(external_luxurygram_shell PROPERTIES
            LUXURYGRAM_SHELL_DLL "${luxurygram_shell_dll}"
        )
    else()
        message(STATUS "luxurygram_shell.dll not found at ${luxurygram_shell_dll}: the GPUI chat surface stays off")
    endif()
endif()
