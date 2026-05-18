---
name: "bimface-helper-v3-3d"
description: "开发基于BIMFACE的Web应用，用于实现在前端页面上对3D模型的可视化和交互。"
version: "1.0.0"
---

# BIMFACE Web应用开发技能

## 使用须知
- 仅支持BIMFACE平台Web应用，使用JSSDK v3版本
- **严格参照** `references/` 目录下对应文档的代码写法，不自行变更接口调用方式
- 仅实现用户明确要求的功能

## 接口调用指南

> **识别同步/异步的方法**：BIMFACE JSSDK v3 的同步/异步区分很简单——方法参数中包含 `callback`/回调函数的即为异步（如 `getComponentProperty`），无回调的即为同步（如 `getBoundingBoxById`）。各 reference 文档中有完整示例，调用时参照示例即可。

### 1. 页面初始化

- BIMFACE初始化：参考[BIMFACE初始化](references/initialize.md)

### 1.1 需求解析（编码前强制执行）

接收用户需求后，先完成以下步骤再开始编码：

1. **能力映射**：将用户需求的关键词对照 `§2 功能模块` 找到匹配的 reference 文档
2. **范围确认**：如果需求涉及 3 个以上模块，列出计划调用的模块清单，让用户确认优先级
3. **不支持告知**：如果需求无法匹配到任何现有 reference，立即告知用户"当前 SDK 能力范围不支持"，避免沉默生成错误代码
4. **多模型判断**：如果需求涉及多个模型/集成模型，确认是否需要 `getModel(modelId)` 指定目标模型

### 2. 功能模块

#### 2.1 场景交互相关
- 常用监听事件：参考[监听事件](references/events.md)
- 路径漫游：参考[路径漫游](references/walkthrough.md)
- 自动旋转：参考[自动旋转](references/autoRotate.md)
- 相机状态：参考[相机状态](references/camera.md)，包括视角切换、相机交互行为和阻尼效果
- 导航地图：参考[导航地图](references/navigationMap.md)
- 自定义工具条：参考[自定义工具条](references/customToolbar.md)

#### 2.2 构件操作相关
- 构件可见性：参考[构件可见性](references/componentVisibility.md)
- 构件着色：参考[构件着色](references/color.md)
- 线框颜色：参考[线框颜色](references/wireframe.md)
- 冻结构件：参考[冻结构件](references/deactivate.md)
- 隔离构件：参考[隔离构件](references/isolate.md)
- 构件强调：参考[构件强调](references/blink.md)
- 模型属性：参考[模型属性](references/property.md)
- 外部构件：参考[外部构件](references/externalObject.md)
- 房间：参考[房间](references/room.md)
- 合模工具：参考[合模工具](references/modelTransformTool.md)，这是一个可供用户在页面调整多个模型之间相互位置的工具
- 模型编辑器：参考[模型编辑器](references/modelEditor.md)
- 模型状态：参考[模型状态](references/modelState.md)
- 轴网：参考[轴网](references/axisGrid.md)

#### 2.3 效果呈现相关
- 扫描效果：参考[扫描效果](references/scanEffect.md)
- 曲线效果：参考[曲线效果](references/curveEffect.md)
- 电子围墙效果：参考[电子围墙效果](references/wallEffect.md)
- 平面扫描效果：参考[平面扫描效果](references/planeScan.md)
- 立体锚点效果：参考[立体锚点效果](references/anchorEffect.md)
- 三维平面：参考[三维平面](references/plane.md)
- 楼层爆炸：参考[楼层爆炸](references/explosion.md)
- 火焰效果：参考[火焰效果](references/fireEffect.md)
- 水面效果：参考[水面效果](references/waterEffect.md)，包括水面、材质流动和喷水效果
- 天气效果：参考[天气效果](references/weatherEffect.md)
- 剖切盒：参考[剖切盒](references/sectionBox.md)
- 剖切面：参考[剖切面](references/sectionPlane.md)
- 生长动画：参考[生长动画](references/growthAnimation.md)
- 天空盒与背景色：参考[天空盒与背景色](references/skyBox.md)
- 光照效果：参考[光照效果](references/light.md)，包括聚光灯、方向光和环境光照(IBL)
- 发光效果：参考[发光效果](references/glowEffect.md)，包括整体发光、轮廓线发光和辉光效果(BloomEffect)
- 显示效果配置：参考[显示效果配置](references/visualization.md)，包括渲染模式、线框、SSAO、接触阴影和镜头光晕
- 飞线效果：参考[飞线效果](references/flyLine.md)
- 材质贴图：参考[材质贴图](references/material.md)，包括构件贴图和Canvas动态材质
- 热力图：参考[热力图](references/heatmap.md)
- 模型内嵌入图纸：参考[模型内嵌入图纸](references/embedDrawing.md)
- 视频效果：参考[视频效果](references/video.md)，包括平面模式和投射模式
- 写实小地图：参考[写实小地图](references/minimap.md)

#### 2.4 空间分析
- 空间分析：参考[空间分析](references/spatialAnalysis.md)，包括可视域分析、控高分析、透视分析、天际线分析和射线检测

#### 2.5 注记相关
- 引线标签：参考[引线标签](references/leadLabel.md)
- 图片标签：参考[图片标签](references/image.md)
- 聚合标签：参考[聚合标签](references/clusterItem.md)
- 模型批注：参考[模型批注](references/annotation.md)
- 模型测量：参考[模型测量](references/measure.md)
- 自定义标签：参考[自定义标签](references/customLabel.md)
- 三维标签：参考[三维标签](references/marker3D.md)
- 尺寸标注：参考[尺寸标注](references/dimension.md)，用于显示模型预制尺寸或程序创建持久线性尺寸标注

#### 2.6 其他
- 构件筛选：参考[构件筛选](references/filter.md)，当接口描述中涉及到根据构件的objectData信息进行筛选时，需要根据筛选条件格式进行传参
- 包围盒（BoundingBox）：参考[包围盒](references/boundingBox.md)，描述如何获取或自行构造包围盒

### 3. 全局变量命名约定
所有reference文档统一使用以下变量名，不再逐个声明：

| 变量名 | 类型/说明 | 获取方式 |
|--------|-----------|----------|
| `viewer3D` | 3D查看器对象 | `app3D.getViewer()` |
| `app3D` | WebApplication应用对象 | `new Glodon.Bimface.Application.WebApplication3D(config)` |
| `model3D` | 3D模型对象 | `viewer3D.getModel()`（单模型）或 `viewer3D.getModel(modelId)`（多模型） |
| `camera3D` | 3D相机对象 | `viewer3D.getCamera()` |
| `drawableContainer` | 标签容器对象 | `new Glodon.Bimface.Plugins.Drawable.DrawableContainer(config)` |

> **关键约束**：`model3D` 必须在 `ViewAdded` 事件触发后才能获取；多模型场景下必须用 `viewer3D.getModel(modelId)` 指定目标模型。

### 4. 代码自查
开发完成后逐项检查：
- **功能回溯**：将生成的代码与原始需求逐条比对，确认需求中的每个功能点都有对应实现，不存在"需要A却生成了B"或额外生成未要求的功能
- 变量名是否与命名约定一致（`viewer3D` / `model3D` / `app3D` / `camera3D` / `drawableContainer`）
- 每个 API 调用是否能在对应 reference 中找到一模一样的方法签名（不自行编造方法名、参数名），如果某个功能在对应 reference 文档中找不到明确的 API，请告知用户"当前 SDK 能力范围不支持"，不要自行编造方法名或参数
- 异步操作（`getComponentProperty`、`getFloors`、`getBoundingBox`、`loadObject` 等）的结果是否在回调函数中处理
- 状态变更后是否调用 `viewer3D.render()`
- 事件处理函数是否为具名函数（确保可注销）
- 筛选条件格式是否与 [构件筛选](references/filter.md) 一致
- 颜色构造是否使用了 `new Glodon.Web.Graphics.Color()` 而非裸颜色字符串或数组
- 添加外部构件后是否调用了 `viewer3D.updateSceneBoundingBox()`（避免构件在相机范围外不可见）
- 代码中是否存在未在 `§3 全局变量命名约定` 中声明的"野生变量"

#### 4.1 常见错误速查
| ❌ 错误 | ✅ 正确 |
|---------|--------|
| `model.setColor(...)` | 不同模块方法名不同：构件着色用 `overrideComponentsColorById`，房间用 `setRoomColor`，线框用 `setWireframeColor` |
| 筛选条件 `{category: "Wall"}` | 正确字段名为 `categoryId`（数字字符串）和 `family`、`levelName`，格式见 [构件筛选](references/filter.md) |
| `viewer.getModel().setColor(...)` | 应使用 `model3D.overrideComponentsColorById(ids, color)` |
| 外部构件移动后不更新包围盒 | 先调用 `viewer3D.updateSceneBoundingBox()` 再 `render()` |
| `markerContainer.onClick(fn)` | `marker.onClick(fn)` onClick 是单个 Marker3D 对象的方法，不是容器的方法 |