# Ps2Godot Releases

Ps2Godot `0.1.1` 是 Godot 4 编辑器插件，用于把 PS2UI Photoshop Package 转换为可编辑的 Control 场景。当前版本已使用 Godot `4.6.2` 验证。

## 前置：安装 PS2UI

本 ZIP 不包含 Photoshop 插件。先从 [PS2UI Releases](https://github.com/Homer79980/PS2UI-Releases/releases/latest) 安装 `PS2UI-Photoshop-<版本>.ccx`，然后在 Photoshop 中导出包含 `layout.json` 和 `sprites/` 的 Package。

## 安装

1. 从 [最新 Release](https://github.com/Homer79980/Ps2Godot-Releases/releases/latest) 下载 `Ps2Godot-0.1.1.zip`。
2. 关闭 Godot 编辑器，把 ZIP 解压到项目根目录。
3. 确认 `project.godot` 与 `addons` 并列，插件路径必须是 `res://addons/ps2godot/`。
4. 打开项目，进入 `Project -> Project Settings -> Plugins`。
5. 启用 `Ps2Godot`。

正确结构：

```text
项目根目录/
  project.godot
  addons/
    ps2godot/
      plugin.cfg
      plugin.gd
      package_importer.gd
      resource_reuse.gd
      font_catalog.gd
```

不要保留 `Ps2Godot-0.1.1/addons/ps2godot` 这样的多余嵌套或第二份插件副本，否则 Godot 全局脚本类缓存可能指向错误目录。

## 导入操作

1. 执行 `Project -> Tools -> Ps2Godot：导入 PS2UI 导出包...`。
2. 选择 Package 根目录，不要选择 `sprites` 子目录。
3. “字体与导入”会列出项目字体、已记住默认和待处理字体；可立即绑定，也可暂用 Godot 默认字体继续。
4. 首次导入会在后台扫描项目 PNG，等待进度结束和“Ps2Godot 导入完成”提示。
5. 在 `res://ps2godot/{module}/scenes` 打开生成的 `.tscn`。

Schema 1 生成一个场景，Schema 2/3 为每个可见画板生成一个场景。普通图片是 `TextureRect`，九宫是 `NinePatchRect`，文字是 `Label`，按钮保留 `Button` 根节点。

## 字体映射

PS2UI Package 已包含当前设计使用的字体身份、字号、行高、字距、对齐和文字矩形，不需要在导入前额外准备字体 JSON。

- 项目字体精确且唯一时可自动匹配；同名候选超过一个时必须人工选择。
- 没有项目字体时仍完整生成场景，文字暂用默认字体并保留待绑定身份。
- 映射保存在 `res://ps2godot/settings/font-map.json`，建议随项目版本控制。
- “高级字体目录”只用于团队共享稳定字体身份和样式，不包含字体文件，也不是日常导入前置条件。

## 项目资源复用

- 解码后的 RGBA 像素与尺寸完全一致：自动复用项目已有纹理，不依赖名称。
- 不同尺寸、镜像或高相似候选：由用户确认是否复用。
- 九宫只做精确且边界安全的复用，不用相似图替换。
- 复用资源保持只读；插件不注册后台资源、导出或构建钩子。
- 指纹与确认结果缓存在项目根目录 `.ps2godot/`。

## 九宫拉伸

选择 `NinePatchRect`，用 2D 布局手柄改变宽高，或在 Inspector 的 Layout 中修改 Size。不要修改节点 `Scale`，否则固定边角也会整体缩放。

## 查看版本

- 在 `Project -> Project Settings -> Plugins` 的 `Ps2Godot` 行查看 Version。
- 或查看 `res://addons/ps2godot/plugin.cfg` 的 `version`。
- Tools 菜单只保留一个中文导入入口，版本号和字体维护不会占用额外菜单。

## 升级、卸载与排错

- 升级：先禁用插件并关闭 Godot，用新版覆盖 `addons/ps2godot`，再启动并重新启用。
- 卸载：禁用插件、关闭编辑器，再删除 `addons/ps2godot`；生成的 `res://ps2godot` 资源可按项目需要保留。
- 点击导入无反应：先确认只有一份 `addons/ps2godot`，再检查 Godot Output 中是否有脚本类缓存或解析错误；删除错误嵌套副本后重启编辑器。
- 重导可能覆盖生成场景，业务脚本应挂在实例化生成场景的外壳场景中。

## 校验下载

```powershell
Get-FileHash .\Ps2Godot-0.1.1.zip -Algorithm SHA256
```

输出应与 `Ps2Godot-0.1.1-SHA256.txt` 一致。
