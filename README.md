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

1. 运行 `Project -> Tools -> Ps2Godot: 导入 Photoshop Package...`。
2. 选择包含 `layout.json` 的 Package 根目录，不要选择 `sprites` 子目录。
3. 在 `res://ps2godot/{module}/scenes` 中打开生成的场景。

## 拉伸九宫

九宫图会导入为 `NinePatchRect`。选择节点后用 2D 布局手柄改变宽高，或在 Inspector 的 Layout 中修改 Size。不要修改节点 `Scale`，否则固定边角也会被整体缩放。

生成场景可能在重导时被覆盖，业务脚本应放在实例化生成场景的外壳场景中。
