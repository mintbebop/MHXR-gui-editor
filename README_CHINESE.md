# MHXR GUI 编辑器

用于编辑 MHXR（怪物猎人：探索，2015）`.gui` 界面文件的编辑器。

基于上游项目 [Fexty12573/mhw-gui-editor](https://github.com/Fexty12573/mhw-gui-editor) 改造。

作者：Mint2333

---

## 目录结构

| 目录 | 说明 |
|---|---|
| `app/` | 编译好的程序，直接运行里面的 `MHXR gui editor.exe` 即可 |
| `source/` | 完整源码（Visual Studio 解决方案） |

## 运行要求

- 64 位 Windows
- 支持 DirectX 11 的显卡

## 使用方法

1. 运行 `app/` 下的 `MHXR gui editor.exe`
2. 首次使用请设置贴图目录：菜单 **工具 > 选项 > ArcFS 目录**，填到包含各个 `GUI_xxx` 界面目录的那一层（例如 `...\arc_cmn\GUI`）。设置后才能正常预览贴图
3. **文件 > 打开**，选择一个 `.gui` 文件（也可以直接把文件拖进窗口）
4. 编辑后 **文件 > 保存**

> 本编辑器只支持 MHXR 版本的 `.gui` 文件。MHW 和 MHGU 的文件打开时会被拒绝并给出提示。

## 功能

- **树状浏览**：动画、对象、序列、对象序列、初始化参数、参数、关键帧、实例、字体、顶点、流程等
- **搜索过滤**：输入关键词回车即可，命中的分类自动展开，没命中的自动隐藏
- **贴图查看器**：10%–300% 缩放，默认自动适配窗口
- **资源管理器**：显示每张贴图的加载状态，失败时给出具体原因（缺 DDS、文件不存在、解码失败等）
- **补丁式保存**：没有改动的部分逐字节原样写回（已通过 roundtrip 校验）
- **界面**：中英文切换，字体大小 8–72 可调，窗口间距会跟着字体一起缩放
- **自动备份**：开启后保存前会生成 `.bak`

## 保存：哪些字段改了会写入文件

| 类别 | 字段 |
|---|---|
| 文件头 | 视图尺寸、attr、instanceID、flowID、variableID、startInstanceIndex、optionBitFlag |
| Objects / Instances | ID、ChildIndex、NextIndex |
| 其它 | Flows、FlowProcesses、Keys、Fonts.ID、Vertices |

**暂不支持保存**（改了不会写入）：

- 所有字符串：名称、贴图路径
- InitParam 与 Param 的数值、关键帧
- Animations、Sequences、ObjectSequences 的各项字段

**关于结构性改动**：如果做了增删条目（新增参数、新增对象等），保存会被拒绝 —— 此时原文件不会被破坏，程序只提示"保存已中止"。

## 从源码编译

1. 用 vcpkg 安装依赖（64 位静态链接）：

```
vcpkg install imgui[core,dx11-binding,win32-binding,docking-experimental] fmt spdlog tomlplusplus nlohmann-json --triplet=x64-windows-static
```

2. 用 Visual Studio 打开 `source/MHXR-GUI-Editor.sln`
3. 选择 **Release | x64** 编译

运行时需要把这些东西放在 exe 同目录：`fonts/`、`themes/`、`lang/`、`data/`、`tegra_swizzle.dll`、`Config.toml`。

## 日志

运行日志写在 exe 同目录的 `editor.log`。程序崩溃时也会把原因记进去，反馈问题时请一并提供这个文件。

## 已知限制

- 仅支持 MHXR 版本，MHW / MHGU 文件无法打开
- 关键帧数值（Param 的 Values）目前不会写回文件
- 选项里的 Chunk 目录、NativePC 目录、KeyValue8/32/128 三项只对 MHW / MHGU 有意义，对 MHXR 无效，界面上已置灰
