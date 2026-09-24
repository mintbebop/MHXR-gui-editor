# MHXR GUI Editor

An editor for the `.gui` files of MHXR (Monster Hunter Explore, 2015).

Based on the upstream project [Fexty12573/mhw-gui-editor](https://github.com/Fexty12573/mhw-gui-editor).

Author: Mint2333

---

## Contents

| Folder | What it is |
|---|---|
| `app/` | Ready to run build, start `MHXR gui editor.exe` inside it |
| `source/` | Full source code (Visual Studio solution) |

## Requirements

- 64-bit Windows
- A graphics card with DirectX 11 support

## Getting started

1. Run `MHXR gui editor.exe` from `app/`
2. On first use set the texture directory: menu **Tools > Options > ArcFS Directory**, point it at the folder that holds the `GUI_xxx` screen folders (for example `...\arc_cmn\GUI`). Textures only show up once this is set
3. **File > Open** and pick a `.gui` file (dragging a file into the window works too)
4. Save with **File > Save**

> Only MHXR `.gui` files are supported. MHW and MHGU files are rejected when opened.

## Features

- **Tree view**: animations, objects, sequences, object sequences, init params, params, keys, instances, fonts, vertices, flows and more
- **Search**: type a keyword and press Enter. Sections with hits open themselves, sections without hits are hidden
- **Texture viewer**: zoom from 10% to 300%, fits the window by default
- **Resource manager**: shows the load state of every texture with a concrete reason when it fails (missing DDS, file not found, decode error, ...)
- **Patch based saving**: everything you did not touch is written back byte for byte identical (roundtrip verified)
- **Interface**: Chinese and English, font size 8 to 72 with spacing following the font
- **Auto backup**: writes a `.bak` before saving when enabled

## Which edits are written back

| Category | Fields |
|---|---|
| Header | view size, attr, instanceID, flowID, variableID, startInstanceIndex, optionBitFlag |
| Objects / Instances | ID, ChildIndex, NextIndex |
| Others | Flows, FlowProcesses, Keys, Fonts.ID, Vertices |

**Not saved** (edits are discarded):

- Every string: names and texture paths
- InitParam and Param values, including keyframes
- All fields of Animations, Sequences and ObjectSequences

**About structural changes**: adding or removing entries makes saving refuse to run. The original file is left untouched in that case, the editor only reports that the save was aborted.

## Building from source

1. Install the dependencies with vcpkg (64-bit static):

```
vcpkg install imgui[core,dx11-binding,win32-binding,docking-experimental] fmt spdlog tomlplusplus nlohmann-json --triplet=x64-windows-static
```

2. Open `source/MHXR-GUI-Editor.sln` with Visual Studio
3. Build **Release | x64**

At runtime these have to sit next to the exe: `fonts/`, `themes/`, `lang/`, `data/`, `tegra_swizzle.dll`, `Config.toml`.

## Log

The log file `editor.log` is written next to the exe. Crashes are recorded there as well, so please attach it when reporting a problem.

## Known limitations

- MHXR only, MHW and MHGU files cannot be opened
- Keyframe values (Param `Values`) are not written back yet
- The Chunk Directory, NativePC Directory and KeyValue8/32/128 options only ever mattered for MHW / MHGU, they do nothing for MHXR and are greyed out
