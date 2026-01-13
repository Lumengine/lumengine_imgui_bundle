cmake_minimum_required(VERSION 3.20)
project(lumengine_imgui_bundle VERSION 1.0.0 LANGUAGES C CXX)

# Set policies
if(POLICY CMP0167)
    cmake_policy(SET CMP0167 NEW)  # Use BoostConfig.cmake
endif()
if(POLICY CMP0169)
    cmake_policy(SET CMP0169 OLD)  # Allow FetchContent_Populate for now
endif()

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# ============================================================================
# Library Git Tags/Commits
# ============================================================================
set(LM_IMGUI_GIT_TAG "74cba1e" CACHE STRING "ImGui git commit/tag")
set(LM_IMPLOT_GIT_TAG "2babf8b" CACHE STRING "ImPlot git commit/tag")
set(LM_IMPLOT3D_GIT_TAG "6a27385" CACHE STRING "ImPlot3D git commit/tag")
set(LM_IMGUI_NODE_EDITOR_GIT_TAG "bb59cc7" CACHE STRING "ImGui Node Editor git commit/tag")
set(LM_IMGUIZMO_GIT_TAG "b2e2e00" CACHE STRING "ImGuizmo git commit/tag")
set(LM_IMGUI_COLOR_TEXT_EDIT_GIT_TAG "e3f369f" CACHE STRING "ImGui Color Text Edit git commit/tag")
set(LM_IMGUI_MD_GIT_TAG "acb65f6" CACHE STRING "ImGui Markdown git commit/tag")
set(LM_MD4C_GIT_TAG "e9ff661" CACHE STRING "md4c git commit/tag")
set(LM_IMGUI_TOGGLE_GIT_TAG "74c4100" CACHE STRING "ImGui Toggle git commit/tag")
set(LM_IMGUI_KNOBS_GIT_TAG "ee9db0b" CACHE STRING "ImGui Knobs git commit/tag")
set(LM_IMGUI_COMMAND_PALETTE_GIT_TAG "ce7ec18" CACHE STRING "ImGui Command Palette git commit/tag")
set(LM_IMGUI_COOL_BAR_GIT_TAG "7e184da" CACHE STRING "ImGui Cool Bar git commit/tag")

# Options to enable/disable each library component
option(LM_IMGUI_BUILD_IMPLOT "Include ImPlot" ON)
option(LM_IMGUI_BUILD_IMPLOT3D "Include ImPlot3D" ON)
option(LM_IMGUI_BUILD_IMGUI_NODE_EDITOR "Include ImGui Node Editor" ON)
option(LM_IMGUI_BUILD_IMGUIZMO "Include ImGuizmo" ON)
option(LM_IMGUI_BUILD_IMGUI_COLOR_TEXT_EDIT "Include ImGui Color Text Edit" ON)
option(LM_IMGUI_BUILD_IMGUI_MD "Include ImGui Markdown" ON)
option(LM_IMGUI_BUILD_IMGUI_TOGGLE "Include ImGui Toggle" ON)
option(LM_IMGUI_BUILD_IMGUI_KNOBS "Include ImGui Knobs" ON)
option(LM_IMGUI_BUILD_IMGUI_COMMAND_PALETTE "Include ImGui Command Palette" ON)
option(LM_IMGUI_BUILD_IMGUI_COOL_BAR "Include ImGui Cool Bar" ON)

# FetchContent setup
include(FetchContent)
set(FETCHCONTENT_QUIET OFF)
set(FETCHCONTENT_UPDATES_DISCONNECTED ON)

# ============================================================================
# Fetch all libraries
# ============================================================================

message(STATUS "Fetching ImGui libraries...")

# ImGui core (always required)
FetchContent_Declare(imgui
    GIT_REPOSITORY https://github.com/pthom/imgui.git
    GIT_TAG ${LM_IMGUI_GIT_TAG}
    GIT_SHALLOW FALSE
)

# ImPlot
if(LM_IMGUI_BUILD_IMPLOT)
    FetchContent_Declare(implot
        GIT_REPOSITORY https://github.com/pthom/implot.git
        GIT_TAG ${LM_IMPLOT_GIT_TAG}
        GIT_SHALLOW FALSE
    )
endif()

# ImPlot3D
if(LM_IMGUI_BUILD_IMPLOT3D)
    FetchContent_Declare(implot3d
        GIT_REPOSITORY https://github.com/pthom/implot3d.git
        GIT_TAG ${LM_IMPLOT3D_GIT_TAG}
        GIT_SHALLOW FALSE
    )
endif()

# ImGui Node Editor
if(LM_IMGUI_BUILD_IMGUI_NODE_EDITOR)
    FetchContent_Declare(imgui_node_editor
        GIT_REPOSITORY https://github.com/pthom/imgui-node-editor.git
        GIT_TAG ${LM_IMGUI_NODE_EDITOR_GIT_TAG}
        GIT_SHALLOW FALSE
    )
endif()

# ImGuizmo
if(LM_IMGUI_BUILD_IMGUIZMO)
    FetchContent_Declare(imguizmo
        GIT_REPOSITORY https://github.com/pthom/ImGuizmo.git
        GIT_TAG ${LM_IMGUIZMO_GIT_TAG}
        GIT_SHALLOW FALSE
    )
endif()

# ImGui Color Text Edit
if(LM_IMGUI_BUILD_IMGUI_COLOR_TEXT_EDIT)
    FetchContent_Declare(imgui_color_text_edit
        GIT_REPOSITORY https://github.com/pthom/ImGuiColorTextEdit.git
        GIT_TAG ${LM_IMGUI_COLOR_TEXT_EDIT_GIT_TAG}
        GIT_SHALLOW FALSE
    )
endif()

# md4c (for ImGui Markdown)
if(LM_IMGUI_BUILD_IMGUI_MD)
    FetchContent_Declare(md4c
        GIT_REPOSITORY https://github.com/mity/md4c.git
        GIT_TAG ${LM_MD4C_GIT_TAG}
        GIT_SHALLOW FALSE
    )

    FetchContent_Declare(imgui_md
        GIT_REPOSITORY https://github.com/pthom/imgui_md.git
        GIT_TAG ${LM_IMGUI_MD_GIT_TAG}
        GIT_SHALLOW FALSE
    )
endif()

# ImGui Toggle
if(LM_IMGUI_BUILD_IMGUI_TOGGLE)
    FetchContent_Declare(imgui_toggle
        GIT_REPOSITORY https://github.com/pthom/imgui_toggle.git
        GIT_TAG ${LM_IMGUI_TOGGLE_GIT_TAG}
        GIT_SHALLOW FALSE
    )
endif()

# ImGui Knobs
if(LM_IMGUI_BUILD_IMGUI_KNOBS)
    FetchContent_Declare(imgui_knobs
        GIT_REPOSITORY https://github.com/pthom/imgui-knobs.git
        GIT_TAG ${LM_IMGUI_KNOBS_GIT_TAG}
        GIT_SHALLOW FALSE
    )
endif()

# ImGui Command Palette
if(LM_IMGUI_BUILD_IMGUI_COMMAND_PALETTE)
    FetchContent_Declare(imgui_command_palette
        GIT_REPOSITORY https://github.com/pthom/imgui-command-palette.git
        GIT_TAG ${LM_IMGUI_COMMAND_PALETTE_GIT_TAG}
        GIT_SHALLOW FALSE
    )
endif()

# ImGui Cool Bar
if(LM_IMGUI_BUILD_IMGUI_COOL_BAR)
    FetchContent_Declare(imgui_cool_bar
        GIT_REPOSITORY https://github.com/pthom/ImCoolBar.git
        GIT_TAG ${LM_IMGUI_COOL_BAR_GIT_TAG}
        GIT_SHALLOW FALSE
    )
endif()

# Make all content available
FetchContent_MakeAvailable(imgui)
if(LM_IMGUI_BUILD_IMPLOT)
    FetchContent_MakeAvailable(implot)
endif()
if(LM_IMGUI_BUILD_IMPLOT3D)
    FetchContent_MakeAvailable(implot3d)
endif()
if(LM_IMGUI_BUILD_IMGUI_NODE_EDITOR)
    FetchContent_MakeAvailable(imgui_node_editor)
endif()
if(LM_IMGUI_BUILD_IMGUIZMO)
    FetchContent_MakeAvailable(imguizmo)
endif()
if(LM_IMGUI_BUILD_IMGUI_COLOR_TEXT_EDIT)
    FetchContent_MakeAvailable(imgui_color_text_edit)
endif()
if(LM_IMGUI_BUILD_IMGUI_MD)
    FetchContent_GetProperties(md4c)
    if(NOT md4c_POPULATED)
        FetchContent_Populate(md4c)
    endif()
    FetchContent_MakeAvailable(imgui_md)
endif()
if(LM_IMGUI_BUILD_IMGUI_TOGGLE)
    FetchContent_MakeAvailable(imgui_toggle)
endif()
if(LM_IMGUI_BUILD_IMGUI_KNOBS)
    FetchContent_MakeAvailable(imgui_knobs)
endif()
if(LM_IMGUI_BUILD_IMGUI_COMMAND_PALETTE)
    FetchContent_MakeAvailable(imgui_command_palette)
endif()
if(LM_IMGUI_BUILD_IMGUI_COOL_BAR)
    FetchContent_GetProperties(imgui_cool_bar)
    if(NOT imgui_cool_bar_POPULATED)
        FetchContent_Populate(imgui_cool_bar)
    endif()
endif()

# ============================================================================
# Collect all source files into a single library
# ============================================================================

set(ALL_SOURCES)
set(ALL_HEADERS)
set(ALL_INCLUDE_DIRS)

# ImGui core
file(GLOB IMGUI_SOURCES ${imgui_SOURCE_DIR}/*.cpp)
file(GLOB IMGUI_HEADERS ${imgui_SOURCE_DIR}/*.h)
list(APPEND ALL_SOURCES ${IMGUI_SOURCES})
list(APPEND ALL_HEADERS ${IMGUI_HEADERS})
list(APPEND ALL_INCLUDE_DIRS ${imgui_SOURCE_DIR})

# ImPlot
if(LM_IMGUI_BUILD_IMPLOT)
    file(GLOB IMPLOT_SOURCES ${implot_SOURCE_DIR}/*.cpp)
    file(GLOB IMPLOT_HEADERS ${implot_SOURCE_DIR}/*.h)
    list(APPEND ALL_SOURCES ${IMPLOT_SOURCES})
    list(APPEND ALL_HEADERS ${IMPLOT_HEADERS})
    list(APPEND ALL_INCLUDE_DIRS ${implot_SOURCE_DIR})
endif()

# ImPlot3D
if(LM_IMGUI_BUILD_IMPLOT3D)
    file(GLOB IMPLOT3D_SOURCES ${implot3d_SOURCE_DIR}/*.cpp)
    file(GLOB IMPLOT3D_HEADERS ${implot3d_SOURCE_DIR}/*.h)
    list(APPEND ALL_SOURCES ${IMPLOT3D_SOURCES})
    list(APPEND ALL_HEADERS ${IMPLOT3D_HEADERS})
    list(APPEND ALL_INCLUDE_DIRS ${implot3d_SOURCE_DIR})
endif()

# ImGui Node Editor
if(LM_IMGUI_BUILD_IMGUI_NODE_EDITOR)
    file(GLOB NODE_EDITOR_SOURCES ${imgui_node_editor_SOURCE_DIR}/*.cpp)
    file(GLOB NODE_EDITOR_HEADERS ${imgui_node_editor_SOURCE_DIR}/*.h ${imgui_node_editor_SOURCE_DIR}/*.inl)
    list(APPEND ALL_SOURCES ${NODE_EDITOR_SOURCES})
    list(APPEND ALL_HEADERS ${NODE_EDITOR_HEADERS})
    list(APPEND ALL_INCLUDE_DIRS ${imgui_node_editor_SOURCE_DIR})
endif()

# ImGuizmo
if(LM_IMGUI_BUILD_IMGUIZMO)
    file(GLOB IMGUIZMO_SOURCES ${imguizmo_SOURCE_DIR}/*.cpp)
    file(GLOB IMGUIZMO_HEADERS ${imguizmo_SOURCE_DIR}/*.h)
    list(APPEND ALL_SOURCES ${IMGUIZMO_SOURCES})
    list(APPEND ALL_HEADERS ${IMGUIZMO_HEADERS})
    list(APPEND ALL_INCLUDE_DIRS ${imguizmo_SOURCE_DIR})
endif()

# ImGui Color Text Edit
if(LM_IMGUI_BUILD_IMGUI_COLOR_TEXT_EDIT)
    list(APPEND ALL_SOURCES
        ${imgui_color_text_edit_SOURCE_DIR}/TextEditor.cpp
        ${imgui_color_text_edit_SOURCE_DIR}/LanguageDefinitions.cpp
    )
    file(GLOB TEXT_EDIT_HEADERS ${imgui_color_text_edit_SOURCE_DIR}/*.h)
    list(APPEND ALL_HEADERS ${TEXT_EDIT_HEADERS})
    list(APPEND ALL_INCLUDE_DIRS
        ${imgui_color_text_edit_SOURCE_DIR}
        ${imgui_color_text_edit_SOURCE_DIR}/vendor/regex/include
    )
endif()

# md4c + ImGui Markdown
if(LM_IMGUI_BUILD_IMGUI_MD)
    list(APPEND ALL_SOURCES
        ${md4c_SOURCE_DIR}/src/md4c.c
        ${md4c_SOURCE_DIR}/src/md4c-html.c
        ${md4c_SOURCE_DIR}/src/entity.c
    )
    file(GLOB MD4C_HEADERS ${md4c_SOURCE_DIR}/src/*.h)
    list(APPEND ALL_HEADERS ${MD4C_HEADERS})

    file(GLOB IMGUI_MD_SOURCES ${imgui_md_SOURCE_DIR}/*.cpp)
    file(GLOB IMGUI_MD_HEADERS ${imgui_md_SOURCE_DIR}/*.h)
    list(APPEND ALL_SOURCES ${IMGUI_MD_SOURCES})
    list(APPEND ALL_HEADERS ${IMGUI_MD_HEADERS})
    list(APPEND ALL_INCLUDE_DIRS ${md4c_SOURCE_DIR}/src ${imgui_md_SOURCE_DIR})
endif()

# ImGui Toggle
if(LM_IMGUI_BUILD_IMGUI_TOGGLE)
    file(GLOB TOGGLE_SOURCES ${imgui_toggle_SOURCE_DIR}/*.cpp)
    file(GLOB TOGGLE_HEADERS ${imgui_toggle_SOURCE_DIR}/*.h)
    list(APPEND ALL_SOURCES ${TOGGLE_SOURCES})
    list(APPEND ALL_HEADERS ${TOGGLE_HEADERS})
    list(APPEND ALL_INCLUDE_DIRS ${imgui_toggle_SOURCE_DIR})
endif()

# ImGui Knobs
if(LM_IMGUI_BUILD_IMGUI_KNOBS)
    file(GLOB KNOBS_SOURCES ${imgui_knobs_SOURCE_DIR}/*.cpp)
    file(GLOB KNOBS_HEADERS ${imgui_knobs_SOURCE_DIR}/*.h)
    list(APPEND ALL_SOURCES ${KNOBS_SOURCES})
    list(APPEND ALL_HEADERS ${KNOBS_HEADERS})
    list(APPEND ALL_INCLUDE_DIRS ${imgui_knobs_SOURCE_DIR})
endif()

# ImGui Command Palette
if(LM_IMGUI_BUILD_IMGUI_COMMAND_PALETTE)
    file(GLOB CMD_PALETTE_SOURCES ${imgui_command_palette_SOURCE_DIR}/*.cpp)
    file(GLOB CMD_PALETTE_HEADERS ${imgui_command_palette_SOURCE_DIR}/*.h)
    list(APPEND ALL_SOURCES ${CMD_PALETTE_SOURCES})
    list(APPEND ALL_HEADERS ${CMD_PALETTE_HEADERS})
    list(APPEND ALL_INCLUDE_DIRS ${imgui_command_palette_SOURCE_DIR})
endif()

# ImGui Cool Bar
if(LM_IMGUI_BUILD_IMGUI_COOL_BAR)
    file(GLOB COOL_BAR_SOURCES ${imgui_cool_bar_SOURCE_DIR}/*.cpp)
    file(GLOB COOL_BAR_HEADERS ${imgui_cool_bar_SOURCE_DIR}/*.h)
    list(APPEND ALL_SOURCES ${COOL_BAR_SOURCES})
    list(APPEND ALL_HEADERS ${COOL_BAR_HEADERS})
    list(APPEND ALL_INCLUDE_DIRS ${imgui_cool_bar_SOURCE_DIR})
endif()

# ============================================================================
# Create single shared library: imgui_bundle
# ============================================================================

add_library(imgui_bundle SHARED ${ALL_SOURCES} ${ALL_HEADERS})

# Remove duplicates from include dirs
list(REMOVE_DUPLICATES ALL_INCLUDE_DIRS)

# Set include directories
target_include_directories(imgui_bundle PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
)

foreach(inc_dir ${ALL_INCLUDE_DIRS})
    target_include_directories(imgui_bundle PUBLIC
        $<BUILD_INTERFACE:${inc_dir}>
    )
endforeach()

# Set compile definitions
# Allow parent projects to override the imconfig.h location
if(NOT DEFINED IMGUI_USER_CONFIG_PATH)
    set(IMGUI_USER_CONFIG_PATH "${CMAKE_CURRENT_SOURCE_DIR}/imconfig.h")
endif()

target_compile_definitions(imgui_bundle
    PUBLIC
        IMGUI_USER_CONFIG="${IMGUI_USER_CONFIG_PATH}"
    PRIVATE
        imgui_bundle_EXPORTS  # Define export macro for DLL building
)

if(LM_IMGUI_BUILD_IMGUI_MD)
    target_compile_definitions(imgui_bundle PRIVATE MD4C_USE_UTF8)
endif()

# On Linux/macOS, set visibility to hidden by default, then export only marked symbols
if(NOT WIN32)
    target_compile_options(imgui_bundle PRIVATE -fvisibility=hidden)
endif()

# Print configuration summary
message(STATUS "=== Lumengine ImGui Bundle Configuration ===")
message(STATUS "Single shared library: imgui_bundle.dll")
message(STATUS "ImPlot: ${LM_IMGUI_BUILD_IMPLOT}")
message(STATUS "ImPlot3D: ${LM_IMGUI_BUILD_IMPLOT3D}")
message(STATUS "ImGui Node Editor: ${LM_IMGUI_BUILD_IMGUI_NODE_EDITOR}")
message(STATUS "ImGuizmo: ${LM_IMGUI_BUILD_IMGUIZMO}")
message(STATUS "ImGui Color Text Edit: ${LM_IMGUI_BUILD_IMGUI_COLOR_TEXT_EDIT}")
message(STATUS "ImGui Markdown: ${LM_IMGUI_BUILD_IMGUI_MD}")
message(STATUS "ImGui Toggle: ${LM_IMGUI_BUILD_IMGUI_TOGGLE}")
message(STATUS "ImGui Knobs: ${LM_IMGUI_BUILD_IMGUI_KNOBS}")
message(STATUS "ImGui Command Palette: ${LM_IMGUI_BUILD_IMGUI_COMMAND_PALETTE}")
message(STATUS "ImGui Cool Bar: ${LM_IMGUI_BUILD_IMGUI_COOL_BAR}")
message(STATUS "==========================================")
