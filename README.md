# Ps2Godot Releases

Ps2Godot 是 Godot 4 编辑器插件，用于把 Photoshop 导出的 PS2UI Package 转换为可编辑 Control 场景。

## 先安装 Photoshop 导出端

Ps2Godot ZIP 不包含 Photoshop 插件。请先从 [PS2UI Photoshop Releases](https://github.com/Homer79980/PSD2Unity-Releases/releases/latest) 下载最新的 `PS2UI-Photoshop-<版本>.ccx`，安装并重启 Photoshop。

在 Photoshop 中打开 `插件 -> PS2UI`，设置模块名与九宫边界并导出 Package。

## 安装

1. 从 [最新 Release](https://github.com/Homer79980/Ps2Godot-Releases/releases/latest) 下载 `Ps2Godot-<版本>.zip`。
2. 把 ZIP 解压到 Godot 工程根目录。
3. 确认插件位于 `res://addons/ps2godot/`。
4. 打开 `Project -> Project Settings -> Plugins` 并启用 `Ps2Godot`。

## 导入

1. 运行 `Project -> Tools -> Ps2Godot 0.1.0: 导入 Photoshop Package...`。
2. 选择包含 `layout.json` 的 Package 根目录，不要选择 `sprites` 子目录。
3. 在 `res://ps2godot/{module}/scenes` 中打开生成的场景。

普通图导入为 `TextureRect`，九宫导入为 `NinePatchRect`，文字导入为 `Label`。没有字体 JSON 或项目字体映射时仍会完整生成场景，并保留 Package 中的布局、字号、行高、对齐和缩放数据。

## 项目资源复用

- 每次手动导入时扫描项目 PNG；解码像素完全相同的资源自动复用，不依赖文件名。
- 不同尺寸、镜像或高相似候选由用户确认；九宫只有四边边界兼容时才复用。
- 不修改被复用的项目资源，也不注册后台资源、导出或构建钩子。

## 字体映射

- `Project -> Tools -> Ps2Godot 0.1.0: 配置项目字体映射...` 可把 Photoshop 字体名绑定到项目字体资源。
- `导入 PS2UI 字体目录...` 和 `导出字体目录给 PS2UI...` 用于团队共享稳定 `fontId`。
- 项目映射保存在 `res://ps2godot/settings/font-map.json`；字体目录是可选增强项，不是导入前置条件。

## 查看版本

在 `Project -> Project Settings -> Plugins` 的 `Ps2Godot` 行查看 Version，或打开 `res://addons/ps2godot/plugin.cfg` 查看 `version`。

## 拉伸九宫

九宫图会导入为 `NinePatchRect`。选择节点后用 2D 布局手柄改变宽高，或在 Inspector 的 Layout 中修改 Size。不要修改节点 `Scale`，否则固定边角也会被整体缩放。

生成场景可能在重导时被覆盖，业务脚本应放在实例化生成场景的外壳场景中。

完整源码和详细说明见 [PSD2Unity-Source](https://github.com/Homer79980/PSD2Unity-Source/tree/main/GodotAddon)。
