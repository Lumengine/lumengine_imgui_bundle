# Lumengine ImGui Bundle

A lightweight, customizable bundle of Dear ImGui and popular ImGui extensions for the Lumengine project. This bundle provides a clean CMake configuration with granular control over which libraries to include, avoiding unnecessary dependencies.

## Features

This bundle includes the following libraries:
- **Dear ImGui** - Core immediate mode GUI library
- **ImPlot** - Advanced plotting library
- **ImPlot3D** - 3D plotting extension
- **ImGui Node Editor** - Node-based editor interface
- **ImGuizmo** - 3D gizmo manipulation
- **ImGui Color Text Edit** - Syntax-highlighted text editor
- **ImGui Markdown** - Markdown rendering
- **ImGui Texture Inspector** - Texture inspection tool
- **ImGui Toggle** - Toggle switches
- **ImGui Knobs** - Knob/dial controls
- **ImGui Command Palette** - Command palette interface
- **ImGui Cool Bar** - Toolbar widget
- **ImFile Dialog** - File dialog

## Usage with FetchContent

Add this to your project's CMakeLists.txt:

```cmake
include(FetchContent)

FetchContent_Declare(
    lumengine_imgui_bundle
    GIT_REPOSITORY https://github.com/YOUR_USERNAME/lumengine_imgui_bundle.git
    GIT_TAG main
)

# Configure which libraries you want (all are ON by default)
set(LM_IMGUI_BUILD_IMGUI ON CACHE BOOL "" FORCE)
set(LM_IMGUI_BUILD_IMPLOT ON CACHE BOOL "" FORCE)
set(LM_IMGUI_BUILD_IMPLOT3D ON CACHE BOOL "" FORCE)
set(LM_IMGUI_BUILD_IMGUI_NODE_EDITOR ON CACHE BOOL "" FORCE)
set(LM_IMGUI_BUILD_IMGUIZMO ON CACHE BOOL "" FORCE)
set(LM_IMGUI_BUILD_IMGUI_COLOR_TEXT_EDIT ON CACHE BOOL "" FORCE)
set(LM_IMGUI_BUILD_IMGUI_MD ON CACHE BOOL "" FORCE)
set(LM_IMGUI_BUILD_IMGUI_TEX_INSPECT ON CACHE BOOL "" FORCE)
set(LM_IMGUI_BUILD_IMGUI_TOGGLE ON CACHE BOOL "" FORCE)
set(LM_IMGUI_BUILD_IMGUI_KNOBS ON CACHE BOOL "" FORCE)
set(LM_IMGUI_BUILD_IMGUI_COMMAND_PALETTE ON CACHE BOOL "" FORCE)
set(LM_IMGUI_BUILD_IMGUI_COOL_BAR ON CACHE BOOL "" FORCE)
set(LM_IMGUI_BUILD_IMFILE_DIALOG ON CACHE BOOL "" FORCE)

FetchContent_MakeAvailable(lumengine_imgui_bundle)

# Link against your target
target_link_libraries(your_target PRIVATE lumengine::lumengine_imgui_bundle)
```

## CMake Options

All options are `ON` by default. Set any to `OFF` to exclude that library:

| Option | Description |
|--------|-------------|
| `LM_IMGUI_BUILD_IMGUI` | Build Dear ImGui core (required by most libraries) |
| `LM_IMGUI_BUILD_IMPLOT` | Build ImPlot |
| `LM_IMGUI_BUILD_IMPLOT3D` | Build ImPlot3D |
| `LM_IMGUI_BUILD_IMGUI_NODE_EDITOR` | Build ImGui Node Editor |
| `LM_IMGUI_BUILD_IMGUIZMO` | Build ImGuizmo |
| `LM_IMGUI_BUILD_IMGUI_COLOR_TEXT_EDIT` | Build ImGui Color Text Edit |
| `LM_IMGUI_BUILD_IMGUI_MD` | Build ImGui Markdown |
| `LM_IMGUI_BUILD_IMGUI_TEX_INSPECT` | Build ImGui Texture Inspector |
| `LM_IMGUI_BUILD_IMGUI_TOGGLE` | Build ImGui Toggle |
| `LM_IMGUI_BUILD_IMGUI_KNOBS` | Build ImGui Knobs |
| `LM_IMGUI_BUILD_IMGUI_COMMAND_PALETTE` | Build ImGui Command Palette |
| `LM_IMGUI_BUILD_IMGUI_COOL_BAR` | Build ImGui Cool Bar |
| `LM_IMGUI_BUILD_IMFILE_DIALOG` | Build ImFile Dialog |

## Building Standalone

```bash
mkdir build
cd build
cmake ..
cmake --build .
```

To disable specific libraries:

```bash
cmake .. -DLM_IMGUI_BUILD_IMPLOT=OFF -DLM_IMGUI_BUILD_IMPLOT3D=OFF
cmake --build .
```

## Requirements

- CMake 3.20 or higher
- C++17 compatible compiler
- Git (for FetchContent)

## Library Sources

All libraries are fetched from pthom's forks at specific commits for stability:
- ImGui: [74cba1e](https://github.com/pthom/imgui.git)
- ImPlot: [2babf8b](https://github.com/pthom/implot.git)
- ImPlot3D: [6a27385](https://github.com/pthom/implot3d.git)
- ImGui Node Editor: [bb59cc7](https://github.com/pthom/imgui-node-editor.git)
- ImGuizmo: [b2e2e00](https://github.com/pthom/ImGuizmo.git)
- ImGui Color Text Edit: [e3f369f](https://github.com/pthom/ImGuiColorTextEdit.git)
- ImGui Markdown: [acb65f6](https://github.com/pthom/imgui_md.git)
- ImGui Tex Inspect: [77dd3e1](https://github.com/pthom/imgui_tex_inspect.git)
- ImGui Toggle: [74c4100](https://github.com/pthom/imgui_toggle.git)
- ImGui Knobs: [ee9db0b](https://github.com/pthom/imgui-knobs.git)
- ImGui Command Palette: [ce7ec18](https://github.com/pthom/imgui-command-palette.git)
- ImCoolBar: [7e184da](https://github.com/pthom/ImCoolBar.git)
- ImFileDialog: [7d9ae51](https://github.com/pthom/ImFileDialog.git)

## License

Each library maintains its own license. Please refer to individual library repositories for licensing information
