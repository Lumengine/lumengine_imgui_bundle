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
# Update these variables to change the version of each library
set(LM_IMGUI_GIT_TAG "74cba1e" CACHE STRING "ImGui git commit/tag")
set(LM_IMPLOT_GIT_TAG "2babf8b" CACHE STRING "ImPlot git commit/tag")
set(LM_IMPLOT3D_GIT_TAG "6a27385" CACHE STRING "ImPlot3D git commit/tag")
set(LM_IMGUI_NODE_EDITOR_GIT_TAG "bb59cc7" CACHE STRING "ImGui Node Editor git commit/tag")
set(LM_IMGUIZMO_GIT_TAG "b2e2e00" CACHE STRING "ImGuizmo git commit/tag")
set(LM_IMGUI_COLOR_TEXT_EDIT_GIT_TAG "e3f369f" CACHE STRING "ImGui Color Text Edit git commit/tag")
set(LM_IMGUI_MD_GIT_TAG "acb65f6" CACHE STRING "ImGui Markdown git commit/tag")
set(LM_MD4C_GIT_TAG "e9ff661" CACHE STRING "md4c git commit/tag")
set(LM_IMGUI_TEX_INSPECT_GIT_TAG "77dd3e1" CACHE STRING "ImGui Texture Inspector git commit/tag")
set(LM_IMGUI_TOGGLE_GIT_TAG "74c4100" CACHE STRING "ImGui Toggle git commit/tag")
set(LM_IMGUI_KNOBS_GIT_TAG "ee9db0b" CACHE STRING "ImGui Knobs git commit/tag")
set(LM_IMGUI_COMMAND_PALETTE_GIT_TAG "ce7ec18" CACHE STRING "ImGui Command Palette git commit/tag")
set(LM_IMGUI_COOL_BAR_GIT_TAG "7e184da" CACHE STRING "ImGui Cool Bar git commit/tag")
set(LM_IMFILE_DIALOG_GIT_TAG "7d9ae51" CACHE STRING "ImFile Dialog git commit/tag")
# ============================================================================

# Options to enable/disable each library
option(LM_IMGUI_BUILD_IMGUI "Build Dear ImGui core" ON)
option(LM_IMGUI_BUILD_IMPLOT "Build ImPlot" ON)
option(LM_IMGUI_BUILD_IMPLOT3D "Build ImPlot3D" ON)
option(LM_IMGUI_BUILD_IMGUI_NODE_EDITOR "Build ImGui Node Editor" ON)
option(LM_IMGUI_BUILD_IMGUIZMO "Build ImGuizmo" ON)
option(LM_IMGUI_BUILD_IMGUI_COLOR_TEXT_EDIT "Build ImGui Color Text Edit (requires Boost)" ON)
option(LM_IMGUI_BUILD_IMGUI_MD "Build ImGui Markdown" ON)
option(LM_IMGUI_BUILD_IMGUI_TEX_INSPECT "Build ImGui Texture Inspector" ON)
option(LM_IMGUI_BUILD_IMGUI_TOGGLE "Build ImGui Toggle" ON)
option(LM_IMGUI_BUILD_IMGUI_KNOBS "Build ImGui Knobs" ON)
option(LM_IMGUI_BUILD_IMGUI_COMMAND_PALETTE "Build ImGui Command Palette" ON)
option(LM_IMGUI_BUILD_IMGUI_COOL_BAR "Build ImGui Cool Bar" ON)
option(LM_IMGUI_BUILD_IMFILE_DIALOG "Build ImFile Dialog" OFF)  # Disabled by default
option(LM_IMGUI_BUILD_MISC_CPP "Build Dear ImGui misc/cpp helpers" ON)
option(LM_IMGUI_ENABLE_BACKENDS "Build Dear ImGui backends" ON)

# Backend defaults: keep conservative to avoid pulling missing deps by default
set(LM_IMGUI_BACKEND_WIN32_DEFAULT OFF)
set(LM_IMGUI_BACKEND_DX11_DEFAULT OFF)
set(LM_IMGUI_BACKEND_OPENGL3_DEFAULT OFF)
set(LM_IMGUI_BACKEND_GLFW_DEFAULT OFF)
set(LM_IMGUI_BACKEND_SDL2_DEFAULT OFF)
if(WIN32)
    set(LM_IMGUI_BACKEND_WIN32_DEFAULT ON)
    if(MSVC)
        set(LM_IMGUI_BACKEND_DX11_DEFAULT ON)
    endif()
endif()

option(LM_IMGUI_BACKEND_WIN32 "Build ImGui Win32 backend" ${LM_IMGUI_BACKEND_WIN32_DEFAULT})
option(LM_IMGUI_BACKEND_DX11 "Build ImGui DirectX11 backend" ${LM_IMGUI_BACKEND_DX11_DEFAULT})
option(LM_IMGUI_BACKEND_OPENGL3 "Build ImGui OpenGL3 backend" ${LM_IMGUI_BACKEND_OPENGL3_DEFAULT})
option(LM_IMGUI_BACKEND_GLFW "Build ImGui GLFW backend" ${LM_IMGUI_BACKEND_GLFW_DEFAULT})
option(LM_IMGUI_BACKEND_SDL2 "Build ImGui SDL2 backend" ${LM_IMGUI_BACKEND_SDL2_DEFAULT})

# FetchContent setup
include(FetchContent)

# Set FetchContent properties for better output
set(FETCHCONTENT_QUIET OFF)
set(FETCHCONTENT_UPDATES_DISCONNECTED ON)

# Directory for cmake modules
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

# Create interface library for the bundle
add_library(lumengine_imgui_bundle INTERFACE)

# ImGui core (required by most other libraries)
if(LM_IMGUI_BUILD_IMGUI)
    message(STATUS "Fetching Dear ImGui...")
    FetchContent_Declare(
        imgui
        GIT_REPOSITORY https://github.com/pthom/imgui.git
        GIT_TAG ${LM_IMGUI_GIT_TAG}
        GIT_SHALLOW FALSE
    )
    FetchContent_MakeAvailable(imgui)

    # Create ImGui library target if not already created
    if(NOT TARGET imgui)
        file(GLOB IMGUI_SOURCES ${imgui_SOURCE_DIR}/*.cpp)
        file(GLOB IMGUI_HEADERS ${imgui_SOURCE_DIR}/*.h)

        set(IMGUI_MISC_SOURCES)
        if(LM_IMGUI_BUILD_MISC_CPP)
            file(GLOB IMGUI_MISC_SOURCES ${imgui_SOURCE_DIR}/misc/cpp/*.cpp)
        endif()

        set(IMGUI_BACKEND_SOURCES)
        set(IMGUI_BACKEND_HEADERS)
        if(LM_IMGUI_ENABLE_BACKENDS)
            if(LM_IMGUI_BACKEND_WIN32)
                list(APPEND IMGUI_BACKEND_SOURCES ${imgui_SOURCE_DIR}/backends/imgui_impl_win32.cpp)
                list(APPEND IMGUI_BACKEND_HEADERS ${imgui_SOURCE_DIR}/backends/imgui_impl_win32.h)
            endif()
            if(LM_IMGUI_BACKEND_DX11)
                list(APPEND IMGUI_BACKEND_SOURCES ${imgui_SOURCE_DIR}/backends/imgui_impl_dx11.cpp)
                list(APPEND IMGUI_BACKEND_HEADERS ${imgui_SOURCE_DIR}/backends/imgui_impl_dx11.h)
            endif()
            if(LM_IMGUI_BACKEND_OPENGL3)
                list(APPEND IMGUI_BACKEND_SOURCES ${imgui_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp)
                list(APPEND IMGUI_BACKEND_HEADERS ${imgui_SOURCE_DIR}/backends/imgui_impl_opengl3.h)
            endif()
            if(LM_IMGUI_BACKEND_GLFW)
                list(APPEND IMGUI_BACKEND_SOURCES ${imgui_SOURCE_DIR}/backends/imgui_impl_glfw.cpp)
                list(APPEND IMGUI_BACKEND_HEADERS ${imgui_SOURCE_DIR}/backends/imgui_impl_glfw.h)
            endif()
            if(LM_IMGUI_BACKEND_SDL2)
                list(APPEND IMGUI_BACKEND_SOURCES ${imgui_SOURCE_DIR}/backends/imgui_impl_sdl2.cpp)
                list(APPEND IMGUI_BACKEND_HEADERS ${imgui_SOURCE_DIR}/backends/imgui_impl_sdl2.h)
            endif()
        endif()

        add_library(imgui STATIC
            ${IMGUI_SOURCES}
            ${IMGUI_HEADERS}
            ${IMGUI_MISC_SOURCES}
            ${IMGUI_BACKEND_SOURCES}
            ${IMGUI_BACKEND_HEADERS}
        )
        target_include_directories(imgui PUBLIC
            $<BUILD_INTERFACE:${imgui_SOURCE_DIR}>
            $<BUILD_INTERFACE:${imgui_SOURCE_DIR}/backends>
            $<BUILD_INTERFACE:${imgui_SOURCE_DIR}/misc/cpp>
            $<INSTALL_INTERFACE:include/imgui>
        )
    endif()

    target_link_libraries(lumengine_imgui_bundle INTERFACE imgui)
endif()

# ImPlot
if(LM_IMGUI_BUILD_IMPLOT)
    message(STATUS "Fetching ImPlot...")
    FetchContent_Declare(
        implot
        GIT_REPOSITORY https://github.com/pthom/implot.git
        GIT_TAG ${LM_IMPLOT_GIT_TAG}
        GIT_SHALLOW FALSE
    )
    FetchContent_MakeAvailable(implot)

    if(NOT TARGET implot)
        file(GLOB IMPLOT_SOURCES ${implot_SOURCE_DIR}/*.cpp)
        file(GLOB IMPLOT_HEADERS ${implot_SOURCE_DIR}/*.h)
        add_library(implot STATIC ${IMPLOT_SOURCES} ${IMPLOT_HEADERS})
        target_include_directories(implot PUBLIC
            $<BUILD_INTERFACE:${implot_SOURCE_DIR}>
            $<INSTALL_INTERFACE:include/implot>
        )
        target_link_libraries(implot PUBLIC imgui)
    endif()

    target_link_libraries(lumengine_imgui_bundle INTERFACE implot)
endif()

# ImPlot3D
if(LM_IMGUI_BUILD_IMPLOT3D)
    message(STATUS "Fetching ImPlot3D...")
    FetchContent_Declare(
        implot3d
        GIT_REPOSITORY https://github.com/pthom/implot3d.git
        GIT_TAG ${LM_IMPLOT3D_GIT_TAG}
        GIT_SHALLOW FALSE
    )
    FetchContent_MakeAvailable(implot3d)

    if(NOT TARGET implot3d)
        file(GLOB IMPLOT3D_SOURCES ${implot3d_SOURCE_DIR}/*.cpp)
        file(GLOB IMPLOT3D_HEADERS ${implot3d_SOURCE_DIR}/*.h)
        add_library(implot3d STATIC ${IMPLOT3D_SOURCES} ${IMPLOT3D_HEADERS})
        target_include_directories(implot3d PUBLIC
            $<BUILD_INTERFACE:${implot3d_SOURCE_DIR}>
            $<INSTALL_INTERFACE:include/implot3d>
        )
        target_link_libraries(implot3d PUBLIC imgui)
    endif()

    target_link_libraries(lumengine_imgui_bundle INTERFACE implot3d)
endif()

# ImGui Node Editor
if(LM_IMGUI_BUILD_IMGUI_NODE_EDITOR)
    message(STATUS "Fetching ImGui Node Editor...")
    FetchContent_Declare(
        imgui_node_editor
        GIT_REPOSITORY https://github.com/pthom/imgui-node-editor.git
        GIT_TAG ${LM_IMGUI_NODE_EDITOR_GIT_TAG}
        GIT_SHALLOW FALSE
    )
    FetchContent_MakeAvailable(imgui_node_editor)

    if(NOT TARGET imgui_node_editor)
        file(GLOB NODE_EDITOR_SOURCES ${imgui_node_editor_SOURCE_DIR}/*.cpp)
        file(GLOB NODE_EDITOR_HEADERS ${imgui_node_editor_SOURCE_DIR}/*.h ${imgui_node_editor_SOURCE_DIR}/*.inl)
        add_library(imgui_node_editor STATIC ${NODE_EDITOR_SOURCES} ${NODE_EDITOR_HEADERS})
        target_include_directories(imgui_node_editor PUBLIC
            $<BUILD_INTERFACE:${imgui_node_editor_SOURCE_DIR}>
            $<INSTALL_INTERFACE:include/imgui_node_editor>
        )
        target_link_libraries(imgui_node_editor PUBLIC imgui)
    endif()

    target_link_libraries(lumengine_imgui_bundle INTERFACE imgui_node_editor)
endif()

# ImGuizmo
if(LM_IMGUI_BUILD_IMGUIZMO)
    message(STATUS "Fetching ImGuizmo...")
    FetchContent_Declare(
        imguizmo
        GIT_REPOSITORY https://github.com/pthom/ImGuizmo.git
        GIT_TAG ${LM_IMGUIZMO_GIT_TAG}
        GIT_SHALLOW FALSE
    )
    FetchContent_MakeAvailable(imguizmo)

    if(NOT TARGET imguizmo)
        file(GLOB IMGUIZMO_SOURCES ${imguizmo_SOURCE_DIR}/*.cpp)
        file(GLOB IMGUIZMO_HEADERS ${imguizmo_SOURCE_DIR}/*.h)
        add_library(imguizmo STATIC ${IMGUIZMO_SOURCES} ${IMGUIZMO_HEADERS})
        target_include_directories(imguizmo PUBLIC
            $<BUILD_INTERFACE:${imguizmo_SOURCE_DIR}>
            $<INSTALL_INTERFACE:include/imguizmo>
        )
        target_link_libraries(imguizmo PUBLIC imgui)
    endif()

    target_link_libraries(lumengine_imgui_bundle INTERFACE imguizmo)
endif()

# ImGui Color Text Edit
if(LM_IMGUI_BUILD_IMGUI_COLOR_TEXT_EDIT)
    message(STATUS "Fetching ImGui Color Text Edit...")
    FetchContent_Declare(
        imgui_color_text_edit
        GIT_REPOSITORY https://github.com/pthom/ImGuiColorTextEdit.git
        GIT_TAG ${LM_IMGUI_COLOR_TEXT_EDIT_GIT_TAG}
        GIT_SHALLOW FALSE
    )
    FetchContent_MakeAvailable(imgui_color_text_edit)

    if(NOT TARGET imgui_color_text_edit)
        file(GLOB TEXT_EDIT_SOURCES
            ${imgui_color_text_edit_SOURCE_DIR}/TextEditor.cpp
            ${imgui_color_text_edit_SOURCE_DIR}/LanguageDefinitions.cpp
        )
        file(GLOB TEXT_EDIT_HEADERS ${imgui_color_text_edit_SOURCE_DIR}/*.h)
        add_library(imgui_color_text_edit STATIC ${TEXT_EDIT_SOURCES} ${TEXT_EDIT_HEADERS})
        target_include_directories(imgui_color_text_edit PUBLIC
            $<BUILD_INTERFACE:${imgui_color_text_edit_SOURCE_DIR}>
            $<BUILD_INTERFACE:${imgui_color_text_edit_SOURCE_DIR}/vendor/regex/include>
            $<INSTALL_INTERFACE:include/imgui_color_text_edit>
        )
        target_link_libraries(imgui_color_text_edit PUBLIC imgui)
        message(STATUS "  Using bundled Boost regex from vendor/regex")
    endif()

    target_link_libraries(lumengine_imgui_bundle INTERFACE imgui_color_text_edit)
endif()

# ImGui Markdown
if(LM_IMGUI_BUILD_IMGUI_MD)
    message(STATUS "Fetching ImGui Markdown...")

    # First fetch md4c dependency (using GetProperties to avoid running its CMakeLists)
    message(STATUS "  Fetching md4c dependency...")
    FetchContent_Declare(
        md4c
        GIT_REPOSITORY https://github.com/mity/md4c.git
        GIT_TAG ${LM_MD4C_GIT_TAG}
        GIT_SHALLOW FALSE
    )
    FetchContent_GetProperties(md4c)
    if(NOT md4c_POPULATED)
        FetchContent_Populate(md4c)
    endif()

    # Build md4c library manually
    if(NOT TARGET md4c)
        file(GLOB MD4C_HEADERS ${md4c_SOURCE_DIR}/src/*.h)
        add_library(md4c STATIC
            ${md4c_SOURCE_DIR}/src/md4c.c
            ${md4c_SOURCE_DIR}/src/md4c-html.c
            ${MD4C_HEADERS}
        )
        target_include_directories(md4c PUBLIC
            $<BUILD_INTERFACE:${md4c_SOURCE_DIR}/src>
            $<INSTALL_INTERFACE:include/md4c>
        )
        target_compile_definitions(md4c PRIVATE MD4C_USE_UTF8)
    endif()

    # Now fetch and build imgui_md
    FetchContent_Declare(
        imgui_md
        GIT_REPOSITORY https://github.com/pthom/imgui_md.git
        GIT_TAG ${LM_IMGUI_MD_GIT_TAG}
        GIT_SHALLOW FALSE
    )
    FetchContent_MakeAvailable(imgui_md)

    if(NOT TARGET imgui_md)
        file(GLOB IMGUI_MD_SOURCES ${imgui_md_SOURCE_DIR}/*.cpp)
        file(GLOB IMGUI_MD_HEADERS ${imgui_md_SOURCE_DIR}/*.h)
        add_library(imgui_md STATIC ${IMGUI_MD_SOURCES} ${IMGUI_MD_HEADERS})
        target_include_directories(imgui_md PUBLIC
            $<BUILD_INTERFACE:${imgui_md_SOURCE_DIR}>
            $<BUILD_INTERFACE:${md4c_SOURCE_DIR}/src>
            $<INSTALL_INTERFACE:include/imgui_md>
        )
        target_link_libraries(imgui_md PUBLIC imgui md4c)
    endif()

    target_link_libraries(lumengine_imgui_bundle INTERFACE imgui_md)
endif()

# ImGui Texture Inspector
if(LM_IMGUI_BUILD_IMGUI_TEX_INSPECT)
    message(STATUS "Fetching ImGui Texture Inspector...")
    FetchContent_Declare(
        imgui_tex_inspect
        GIT_REPOSITORY https://github.com/pthom/imgui_tex_inspect.git
        GIT_TAG ${LM_IMGUI_TEX_INSPECT_GIT_TAG}
        GIT_SHALLOW FALSE
    )
    FetchContent_MakeAvailable(imgui_tex_inspect)

    if(NOT TARGET imgui_tex_inspect)
        # Only build the main library, exclude demo
        file(GLOB IMGUI_TEX_INSPECT_HEADERS ${imgui_tex_inspect_SOURCE_DIR}/*.h)
        add_library(imgui_tex_inspect STATIC
            ${imgui_tex_inspect_SOURCE_DIR}/imgui_tex_inspect.cpp
            ${IMGUI_TEX_INSPECT_HEADERS}
        )
        target_include_directories(imgui_tex_inspect PUBLIC
            $<BUILD_INTERFACE:${imgui_tex_inspect_SOURCE_DIR}>
            $<INSTALL_INTERFACE:include/imgui_tex_inspect>
        )
        target_link_libraries(imgui_tex_inspect PUBLIC imgui)
    endif()

    target_link_libraries(lumengine_imgui_bundle INTERFACE imgui_tex_inspect)
endif()

# ImGui Toggle
if(LM_IMGUI_BUILD_IMGUI_TOGGLE)
    message(STATUS "Fetching ImGui Toggle...")
    FetchContent_Declare(
        imgui_toggle
        GIT_REPOSITORY https://github.com/pthom/imgui_toggle.git
        GIT_TAG ${LM_IMGUI_TOGGLE_GIT_TAG}
        GIT_SHALLOW FALSE
    )
    FetchContent_MakeAvailable(imgui_toggle)

    if(NOT TARGET imgui_toggle)
        file(GLOB IMGUI_TOGGLE_SOURCES ${imgui_toggle_SOURCE_DIR}/*.cpp)
        file(GLOB IMGUI_TOGGLE_HEADERS ${imgui_toggle_SOURCE_DIR}/*.h)
        add_library(imgui_toggle STATIC ${IMGUI_TOGGLE_SOURCES} ${IMGUI_TOGGLE_HEADERS})
        target_include_directories(imgui_toggle PUBLIC
            $<BUILD_INTERFACE:${imgui_toggle_SOURCE_DIR}>
            $<INSTALL_INTERFACE:include/imgui_toggle>
        )
        target_link_libraries(imgui_toggle PUBLIC imgui)
    endif()

    target_link_libraries(lumengine_imgui_bundle INTERFACE imgui_toggle)
endif()

# ImGui Knobs
if(LM_IMGUI_BUILD_IMGUI_KNOBS)
    message(STATUS "Fetching ImGui Knobs...")
    FetchContent_Declare(
        imgui_knobs
        GIT_REPOSITORY https://github.com/pthom/imgui-knobs.git
        GIT_TAG ${LM_IMGUI_KNOBS_GIT_TAG}
        GIT_SHALLOW FALSE
    )
    FetchContent_MakeAvailable(imgui_knobs)

    if(NOT TARGET imgui_knobs)
        file(GLOB IMGUI_KNOBS_SOURCES ${imgui_knobs_SOURCE_DIR}/*.cpp)
        file(GLOB IMGUI_KNOBS_HEADERS ${imgui_knobs_SOURCE_DIR}/*.h)
        add_library(imgui_knobs STATIC ${IMGUI_KNOBS_SOURCES} ${IMGUI_KNOBS_HEADERS})
        target_include_directories(imgui_knobs PUBLIC
            $<BUILD_INTERFACE:${imgui_knobs_SOURCE_DIR}>
            $<INSTALL_INTERFACE:include/imgui_knobs>
        )
        target_link_libraries(imgui_knobs PUBLIC imgui)
    endif()

    target_link_libraries(lumengine_imgui_bundle INTERFACE imgui_knobs)
endif()

# ImGui Command Palette
if(LM_IMGUI_BUILD_IMGUI_COMMAND_PALETTE)
    message(STATUS "Fetching ImGui Command Palette...")
    FetchContent_Declare(
        imgui_command_palette
        GIT_REPOSITORY https://github.com/pthom/imgui-command-palette.git
        GIT_TAG ${LM_IMGUI_COMMAND_PALETTE_GIT_TAG}
        GIT_SHALLOW FALSE
    )
    FetchContent_MakeAvailable(imgui_command_palette)

    if(NOT TARGET imgui_command_palette)
        file(GLOB CMD_PALETTE_SOURCES ${imgui_command_palette_SOURCE_DIR}/*.cpp)
        file(GLOB CMD_PALETTE_HEADERS ${imgui_command_palette_SOURCE_DIR}/*.h)
        add_library(imgui_command_palette STATIC ${CMD_PALETTE_SOURCES} ${CMD_PALETTE_HEADERS})
        target_include_directories(imgui_command_palette PUBLIC
            $<BUILD_INTERFACE:${imgui_command_palette_SOURCE_DIR}>
            $<INSTALL_INTERFACE:include/imgui_command_palette>
        )
        target_link_libraries(imgui_command_palette PUBLIC imgui)
    endif()

    target_link_libraries(lumengine_imgui_bundle INTERFACE imgui_command_palette)
endif()

# ImGui Cool Bar
if(LM_IMGUI_BUILD_IMGUI_COOL_BAR)
    message(STATUS "Fetching ImGui Cool Bar...")
    FetchContent_Declare(
        imgui_cool_bar
        GIT_REPOSITORY https://github.com/pthom/ImCoolBar.git
        GIT_TAG ${LM_IMGUI_COOL_BAR_GIT_TAG}
        GIT_SHALLOW FALSE
    )
    FetchContent_GetProperties(imgui_cool_bar)
    if(NOT imgui_cool_bar_POPULATED)
        FetchContent_Populate(imgui_cool_bar)
    endif()

    if(NOT TARGET imgui_cool_bar)
        file(GLOB COOL_BAR_SOURCES ${imgui_cool_bar_SOURCE_DIR}/*.cpp)
        file(GLOB COOL_BAR_HEADERS ${imgui_cool_bar_SOURCE_DIR}/*.h)
        add_library(imgui_cool_bar STATIC ${COOL_BAR_SOURCES} ${COOL_BAR_HEADERS})
        target_include_directories(imgui_cool_bar PUBLIC
            $<BUILD_INTERFACE:${imgui_cool_bar_SOURCE_DIR}>
            $<INSTALL_INTERFACE:include/imgui_cool_bar>
        )
        target_link_libraries(imgui_cool_bar PUBLIC imgui)
    endif()

    target_link_libraries(lumengine_imgui_bundle INTERFACE imgui_cool_bar)
endif()

message(STATUS "=== Lumengine ImGui Bundle Configuration ===")
message(STATUS "ImGui: ${LM_IMGUI_BUILD_IMGUI}")
message(STATUS "ImPlot: ${LM_IMGUI_BUILD_IMPLOT}")
message(STATUS "ImPlot3D: ${LM_IMGUI_BUILD_IMPLOT3D}")
message(STATUS "ImGui Node Editor: ${LM_IMGUI_BUILD_IMGUI_NODE_EDITOR}")
message(STATUS "ImGuizmo: ${LM_IMGUI_BUILD_IMGUIZMO}")
message(STATUS "ImGui Color Text Edit: ${LM_IMGUI_BUILD_IMGUI_COLOR_TEXT_EDIT}")
message(STATUS "ImGui Markdown: ${LM_IMGUI_BUILD_IMGUI_MD}")
message(STATUS "ImGui Texture Inspector: ${LM_IMGUI_BUILD_IMGUI_TEX_INSPECT}")
message(STATUS "ImGui Toggle: ${LM_IMGUI_BUILD_IMGUI_TOGGLE}")
message(STATUS "ImGui Knobs: ${LM_IMGUI_BUILD_IMGUI_KNOBS}")
message(STATUS "ImGui Command Palette: ${LM_IMGUI_BUILD_IMGUI_COMMAND_PALETTE}")
message(STATUS "ImGui Cool Bar: ${LM_IMGUI_BUILD_IMGUI_COOL_BAR}")
message(STATUS "ImFile Dialog: ${LM_IMGUI_BUILD_IMFILE_DIALOG}")
message(STATUS "==========================================")
