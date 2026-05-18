---
name: "bimface-helper-v3-2d"
description: "开发基于BIMFACE的Web应用，用于实现在前端页面上对二维矢量图纸的可视化和交互。"
version: "1.0.0"
---

# BIMFACE 矢量图纸 Web应用开发技能

## 使用须知
- 仅支持BIMFACE平台Web应用，使用JSSDK v3版本
- **严格参照** `references/` 目录下对应文档的代码写法，不自行变更接口调用方式
- 仅实现用户明确要求的功能

## 接口调用指南

### 1. 页面初始化

- BIMFACE初始化：参考[BIMFACE初始化](references/initialize.md)

### 1.1 需求解析（编码前强制执行）

接收用户需求后，先完成以下步骤再开始编码：

1. **能力映射**：将用户需求的关键词对照 `§2 功能模块` 找到匹配的 reference 文档
2. **范围确认**：如果需求涉及 3 个以上模块，列出计划调用的模块清单，让用户确认优先级
3. **不支持告知**：如果需求无法匹配到任何现有 reference，立即告知用户"当前 SDK 能力范围不支持"，避免沉默生成错误代码
4. **多图纸判断**：如果需求涉及切换或同时加载多张图纸，确认是否需要通过 `drawing2D.getDrawing(modelId)` 指定目标图纸

### 2. 功能模块

#### 2.1 图纸交互相关
- 常用监听事件：参考[监听事件](references/events.md)
- 显示模式与视图切换：参考[显示模式与视图](references/displayMode.md)
- 视图控制：参考[视图控制](references/camera.md)
- 状态保存与恢复：参考[状态管理](references/state.md)
- 图纸截图：参考[截图](references/snapshot.md)

#### 2.2 图元操作相关
- 图元选中/高亮/隐藏/显示：参考[图元操作](references/element.md)
- 包围盒：参考[包围盒](references/boundingBox.md)

#### 2.3 图层相关
- 图层管理：参考[图层管理](references/layer.md)

#### 2.4 注记相关
- 二维标签：参考[二维标签](references/drawable.md)
- 聚合标签：参考[聚合标签](references/clusterItem.md)
- 图纸批注：参考[图纸批注](references/annotation.md)
- 图纸测量：参考[图纸测量](references/measure.md)

### 3. 全局变量命名约定
所有reference文档统一使用以下变量名，不再逐个声明：

| 变量名 | 类型/说明 | 获取方式 |
|--------|-----------|----------|
| `viewer2D` | 二维图纸查看器对象 | `app2D.getViewer()` |
| `app2D` | WebApplicationDrawing应用对象 | `new Glodon.Bimface.Application.WebApplicationDrawing(config)` |
| `drawing2D` | 图纸对象 | `viewer2D.getDrawing(modelId)`（不传modelId则返回默认图纸对象） |
| `drawableContainer` | 二维标签容器对象 | `new Glodon.Bimface.Plugins.Drawable.DrawableContainer(config)` |

> **关键约束**：`drawing2D` 必须在图纸 `Loaded` 事件触发后才能获取；操作图层/图元前需通过 `viewer2D.getDrawing(modelId)` 获取目标图纸对象。

### 4. 代码自查
开发完成后逐项检查：
- **功能回溯**：将生成的代码与原始需求逐条比对，确认需求中的每个功能点都有对应实现，不存在"需要A却生成了B"或额外生成未要求的功能
- 变量名是否与命名约定一致（`viewer2D` / `app2D` / `drawing2D` / `drawableContainer`）
- 每个 API 调用是否能在对应 reference 中找到一模一样的方法签名（不自行编造方法名、参数名），如果某个功能在对应 reference 文档中找不到明确的 API，请告知用户"当前 SDK 能力范围不支持"，不要自行编造方法名或参数
- 异步操作（`getObjectDataById`、`getAllObjectIds`、`createSnapshotAsync` 等）的结果是否在回调函数中处理
- 状态变更后是否调用 `viewer2D.render()`
- 事件处理函数是否为具名函数（确保可注销）
- 图纸操作前是否已通过 `Loaded` 事件确认图纸加载完毕
- 颜色构造是否使用了 `new Glodon.Web.Graphics.Color()` 而非裸颜色字符串或数组
- 代码中是否存在未在 `§3 全局变量命名约定` 中声明的"野生变量"

#### 4.1 常见错误速查
| ❌ 错误 | ✅ 正确 |
|---------|--------|
| `new WebApplication3DConfig()` + `WebApplication3D` | 矢量图纸应使用 `WebApplicationDrawingConfig` + `WebApplicationDrawing`，详见 [BIMFACE初始化](references/initialize.md) |
| `viewMetaData.viewType == "3DView"` | 矢量图纸应判断 `viewMetaData.viewType == "dwgView"`，详见 [BIMFACE初始化](references/initialize.md) |
| 在 `ViewAdded` 事件中操作图元 | 二维图纸应监听 `Loaded` 事件而非 `ViewAdded`，详见 [监听事件](references/events.md) |
| 二维坐标传入 `{x, y, z}` | 图纸的世界坐标为 `{x, y}` 不含 z 值，传入 z 值会被忽略或报错 |
| `viewer2D.getObjectDataById(id, cb)` | 应使用 `drawing2D.getObjectDataById(id, cb)`，该方法是图纸对象的方法，不是 viewer 的方法 |
