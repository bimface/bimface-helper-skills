---
name: "bimface-helper-v4-3d"
description: "开发基于BIMFACE的Web应用，用于实现在前端页面上对3D模型的可视化和交互。基于JSSDK v4版本，大量接口采用异步Promise模式。"
---

# BIMFACE v4 Web应用开发技能 - 3D

## 使用须知
- 仅支持BIMFACE平台Web应用，使用JSSDK v4版本
- **严格参照** `references/` 目录下对应文档的代码写法，不自行变更接口调用方式
- 仅实现用户明确要求的功能

## v4 版本核心变化

v4 相比 v3 的核心变化，编码时必须特别注意：

1. **ESM 模块导入**：SDK 通过 ESM import 加载（`await import('...BimfaceSDKLoader-v4.esm.js')`），也可使用 script 标签 fallback
2. **异步优先**：大量接口从同步/回调改为返回 Promise（如 `getComponentProperty`、`getFloors`、`getBoundingBoxByIds`、`getMatchIds`、`setStatus`、`setStandardView`、`zoomToBoundingBox`、`getStatus` 等），必须使用 `.then()` 或 `async/await` 处理结果
3. **Config 类去除**：所有 `*Config` 类均被移除，构造函数直接接收对象参数
4. **命名空间变更**：`Plugins` → `Plugin`（单数）；`Color` 类路径变更为 `Glodon.Bimface.Common.Graphics.Color`
5. **Widget 模式**：内置工具通过 `app.initializeWidget("WidgetName")` + `app.getWidget("WidgetName")` 获取

## 接口调用指南

### 1. 页面初始化

- SDK加载与应用初始化：参考[初始化](references/initialize.md)

### 1.1 需求解析（编码前强制执行）

接收用户需求后，先完成以下步骤再开始编码：

1. **能力映射**：将用户需求的关键词对照 `§2 功能模块` 找到匹配的 reference 文档
2. **范围确认**：如果需求涉及 3 个以上模块，列出计划调用的模块清单，让用户确认优先级
3. **不支持告知**：如果需求无法匹配到任何现有 reference，立即告知用户"当前 SDK 能力范围不支持"，避免沉默生成错误代码
4. **多模型判断**：如果需求涉及多个模型/集成模型，确认是否需要 `viewer.getModel(modelId)` 指定目标模型

### 2. 功能模块

#### 2.1 场景交互相关
- 常用监听事件：参考[监听事件](references/events.md)
- 相机状态与视角：参考[相机](references/camera.md)，包括视角切换、交互行为、缩放到包围盒/选中构件
- 导航模式：参考[导航模式](references/navigation.md)
- 路径漫游：参考[路径漫游](references/walkthrough.md)

#### 2.2 构件操作相关
- 构件可见性：参考[构件可见性](references/componentVisibility.md)
- 构件着色：参考[构件着色](references/color.md)
- 构件高亮：参考[构件高亮](references/highlight.md)
- 构件隔离：参考[隔离构件](references/isolate.md)
- 模型属性：参考[模型属性](references/property.md)
- 构件选中：参考[构件选中](references/selection.md)
- 包围盒：参考[包围盒](references/boundingBox.md)
- 轴网：参考[轴网](references/axisGrid.md)
- 模型变换：参考[模型变换](references/transformation.md)
- 图纸关联：参考[图纸关联](references/embedDrawing.md)
- 视角定位：参考[视角定位](references/zoomTo.md)

#### 2.3 效果呈现相关
- 模型爆炸：参考[模型爆炸](references/explosion.md)
- 剖切盒：参考[剖切盒](references/sectionBox.md)
- 剖切面：参考[剖切面](references/sectionPlane.md)
- 火焰效果：参考[火焰效果](references/fire.md)
- 电子围墙效果：参考[电子围墙效果](references/wallEffect.md)

#### 2.4 注记相关
- 三维标签：参考[三维标签](references/marker3D.md)
- 自定义标签：参考[自定义标签](references/customItem.md)
- 引线标签：参考[引线标签](references/leadLabel.md)
- Drawable容器：参考[Drawable容器](references/drawableContainer.md)
- 聚合标签：参考[聚合标签](references/cluster.md)
- 立体锚点效果：参考[立体锚点效果](references/anchor.md)

#### 2.5 工具相关
- 模型批注：参考[模型批注](references/annotation.md)
- 模型测量：参考[模型测量](references/measure.md)
- 合模工具：参考[合模工具](references/modelTransformTool.md)
- 模型编辑器：参考[模型编辑器](references/modelEditor.md)
- 导航地图：参考[导航地图](references/navigationMap.md)

#### 2.6 空间分析
- 房间：参考[房间](references/room.md)
- 房间管理：参考[房间管理](references/roomManager.md)

#### 2.7 外部资源
- 外部构件：参考[外部构件](references/externalObject.md)

#### 2.8 其他
- 构件筛选：参考[构件筛选](references/filter.md)，当接口描述中涉及到根据构件的objectData信息进行筛选时，需要根据筛选条件格式进行传参

### 3. 全局变量命名约定
所有reference文档统一使用以下变量名，不再逐个声明：

| 变量名 | 类型/说明 | 获取方式 |
|--------|-----------|----------|
| `viewer3D` | 3D查看器对象 | `app.createViewer({...})` |
| `app` | WebApplication应用对象 | `new Glodon.Bimface.Application.WebApplication3D({domElement, tool:{...}})` |
| `model` | 3D模型对象 | `viewer3D.getModel()`（单模型）或 `viewer3D.getModel(modelId)`（多模型） |
| `camera` | 3D相机对象 | `viewer3D.getCamera()` |

> **关键约束**：`model` 必须在模型加载完成后才能获取；多模型场景下必须用 `viewer3D.getModel(modelId)` 指定目标模型。

### 4. 代码自查
开发完成后逐项检查：
- **功能回溯**：将生成的代码与原始需求逐条比对，确认需求中的每个功能点都有对应实现，不存在"需要A却生成了B"或额外生成未要求的功能
- 变量名是否与命名约定一致（`viewer3D` / `model` / `app` / `camera`）
- 每个 API 调用是否能在对应 reference 中找到一模一样的方法签名（不自行编造方法名、参数名），如果某个功能在对应 reference 文档中找不到明确的 API，请告知用户"当前 SDK 能力范围不支持"
- **异步 Promise**：`getComponentProperty`、`getFloors`、`getBoundingBoxByIds`、`getMatchIds`、`getDrawingList`、`setStatus`、`setStandardView`、`zoomToBoundingBox`、`getStatus`（SectionBox/SectionPlane/Annotation/ModelEditor）、`getProperty`（Room）、`getComponents`（Room）、`getAreas`（Model）等返回 Promise 的方法，是否通过 `.then()` 或 `async/await` 处理结果
- 颜色构造是否使用了 `new Glodon.Bimface.Common.Graphics.Color()` 而非 `Glodon.Web.Graphics.Color`
- 插件命名空间是否使用了 `Plugin`（单数）而非 `Plugins`
- 房间类是否使用了 `Glodon.Bimface.Plugin.Room.RoomItem` 而非 `Room`
- 房间管理器是否通过 `viewer3D.getRoomManager()` 获取而非 `new RoomManager()`
- 事件监听是否使用了 `addEventListener` / `removeEventListener` 模式
- 构件操作（visibility/color/opacity/selection）的参数是否使用了对象形式（如 `{ids: [...], all: true}`）而非裸数组
- 模型加载方式是否为 `viewer3D.loadModel({viewMetaData})` 而非 `viewer3D.addView(viewToken)`

#### 4.1 常见错误速查
| ❌ 错误 | ✅ 正确 |
|---------|--------|
| `model.setColor(...)` | 构件着色用 `overrideComponentColor`，房间用 `setColor` |
| `viewer3D.addView(viewToken)` | 应使用 `viewer3D.loadModel({viewMetaData/modelId})` |
| `model.hideComponentsById(ids)` | 应使用 `model.hideComponents({ids: componentIds})` |
| `model.getComponentProperty(id, callback)` | 应使用 `model.getComponentProperty(id).then(property => {...})` |
| `new Glodon.Web.Graphics.Color(255,0,0,1)` | 应使用 `new Glodon.Bimface.Common.Graphics.Color(255,0,0,1)` |
| `Glodon.Bimface.Plugins.Marker3D.Marker3D` | 应使用 `Glodon.Bimface.Plugin.Marker3D.Marker3DItem` |
| `new Glodon.Bimface.Plugin.Room.Room(...)` | 应使用 `new Glodon.Bimface.Plugin.Room.RoomItem({...})` |
| `new RoomManager({viewer})` | 应使用 `viewer3D.getRoomManager()` |
| `sectionBox.exit()` | 应使用 `sectionBoxTool.switchOff()` |
| `measure.setUnits({...})` | 应使用 `measureTool.setResultFormat({...})` |
| `camera.setView(...)` | 应使用 `camera.setStandardView(...)` |
| `camera.setState(state)` | 应使用 `camera.setStatus(state).then(...)` |
