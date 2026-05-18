# Viewer3D中的监听事件

## 事件触发顺序

BIMFACE JSSDK v3 的事件按以下顺序触发，**请严格遵守此顺序**进行初始化代码编写：

| 顺序 | 事件名称 | 触发时机 | 传递参数 | 可用对象 |
|------|---------|---------|---------|---------|
| 1 | `ViewAdded` | 场景加载完成（全局只触发一次） | 无 | `viewer3D`、`model3D`、可初始化 `DrawableContainer` |
| 2 | `ModelAdded` | 每个模型加载完成后触发 | `modelId`（字符串） | `viewer3D`（`model3D` 此时也已就绪） |
| 3 | `ComponentsHoverChanged` | 鼠标悬浮构件变化 | `{objectId}` | `viewer3D`、`model3D` |
| 4 | `Rendered` | 每帧渲染完成后 | 无 | `viewer3D`、`model3D` |

> ⚠️ **时序要点**：`ViewAdded` 先触发（全局一次），此时 `model3D` 已就绪，可以初始化 `DrawableContainer`；`ModelAdded` 随后在每个模型加载完毕后触发（多模型场景会触发多次），传递 `modelId` 可用于 `viewer3D.getModel(modelId)` 操作特定模型。

## 使用约束与说明
- 具名函数原则：事件处理函数必须是一个具名函数，不能是匿名函数，否则注册的事件无法被注销
- 严格按照下方文档说明的数据结构获取事件参数，不要捏造不存在的属性
- 代码块中的eventHandler是一个示例函数，不意味着每个事件的处理函数都是一样的函数名称或内容

## 添加和注销事件监听器（基础范式）
```JavaScript
// 定义事件处理函数
const eventHandler = function (eventData) {
  console.log("事件触发", eventData);
}

// 添加事件监听器
viewer3D.addEventListener('eventName', eventHandler);

// 注销事件监听器
viewer3D.removeEventListener('eventName', eventHandler);
```

## 场景加载完成事件
```JavaScript
// 场景加载完成事件处理函数
viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded, eventHandler);
```

## 模型加载完成事件
模型加载完成事件触发后，会返回一个字符串，字符串内容为模型的ID，您可以使用该ID来操作该模型。
```JavaScript
// 定义事件处理函数
const eventHandler = function (eventData) {
  model3D = viewer3D.getModel(eventData);
}

// 模型加载完成事件处理函数
viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelAdded, eventHandler);
```
## 鼠标点击事件
```JavaScript
// 定义事件处理函数
const eventHandler = function (eventData) {
  // 防御性判断：如果没有点中构件，则不执行操作
  if (!eventData.objectId) {
    return;
  }
  // 点击事件处理逻辑
  console.log("点击事件触发", eventData);
}

// 鼠标点击事件处理函数
viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.MouseClicked, eventHandler);
```
点击事件触发后，会返回一个对象，对象包含以下属性：
- `eventType`：事件类型，固定为`Click`或`RightClick`，分别表示左键点击和右键点击
- `modelId`：点击的模型ID，只有点击在模型上时才会返回，否则没有该属性
- `objectId`：点击的构件ID，只有点击在模型上时才会返回，否则没有该属性
- `worldPosition`：鼠标点击位置的三维坐标，格式为`{x: 100, y: 200, z: 300}`，同样只有点击在模型上时才会返回，否则没有该属性
- `normal`：点击位置的法线向量，格式为`{x: 0.1, y: 0.2, z: 0.3}`，只有点击在模型上时才会返回，否则该属性为null
- `boundingBox`：点击构件的包围盒信息，格式为`{min: {x: 100, y: 200, z: 300}, max: {x: 200, y: 300, z: 400}}`，只有点击在模型上时才会返回，否则没有该属性

## 鼠标双击事件
```JavaScript
// 定义事件处理函数
const eventHandler = function (eventData) {
  // 防御性判断：如果没有点中构件，则不执行操作
  if (!eventData.objectId) {
    return;
  }
  // 双击事件处理逻辑
  console.log("双击事件触发", eventData);
}

// 鼠标双击事件处理函数
viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.MouseDoubleClicked, eventHandler);
```
双击事件触发后，会返回一个对象，对象包含以下属性：
- `eventType`：事件类型，固定为`DoubleClick`，表示左键双击
- `modelId`：双击的模型ID，只有双击在模型上时才会返回，否则没有该属性
- `objectId`：双击的构件ID，只有双击在模型上时才会返回，否则没有该属性
- `worldPosition`：鼠标双击位置的三维坐标，格式为`{x: 100, y: 200, z: 300}`，同样只有双击在模型上时才会返回，否则没有该属性
- `normal`：双击位置的法线向量，格式为`{x: 0.1, y: 0.2, z: 0.3}`，只有双击在模型上时才会返回，否则该属性为null

## 构件选择事件
构件选择范围发生变化后，会触发构件选中事件，并返回一个对象，其中每一组键值对中的key代表modelId，value代表用户选中的该模型下的构件ID列表。例如：

```JSON
{
  "10000909527733": ["393020", "390274", "390260"],
  "10000909527734": ["393021", "390275", "390261"]
}
```
其含义为：
- 10000909527733模型下选中的构件ID列表为["393020", "390274", "390260"]
- 10000909527734模型下选中的构件ID列表为["393021", "390275", "390261"]

```JavaScript
// 定义事件处理函数
const eventHandler = function (eventData) {
  // 遍历eventData对象，将每个模型下的选中构件进行隐藏
  Object.entries(eventData).forEach(([modelId, objectIds]) => {
    // 逐一获取model对象，并隐藏选中的构件
    viewer3D.getModel(modelId).hideComponentsById(objectIds);
    // 视图发生状态变更后，建议调用render刷新
    viewer3D.render();
  });
}

// 构件选择事件处理函数，将所选构件进行隐藏
viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.SelectionChangedInModel, eventHandler)
```

## 渲染完成事件

场景每帧渲染完成后触发，可用于同步多个 viewer 的相机状态。

```javascript
function onRendered() {
    // 渲染完成后执行（如将当前状态同步到另一个 viewer）
    const state = viewer3D.getCurrentState();
    anotherViewer.setState(state);
}

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.Rendered, onRendered);

// 取消监听
viewer3D.removeEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.Rendered, onRendered);
```

## 构件悬浮变化事件

鼠标悬浮的构件发生变化时触发，事件参数 `{objectId}` 为当前悬浮的构件ID。

```javascript
function onHoverChanged(eventData) {
    // eventData.objectId — 当前悬浮的构件ID，无悬浮时为 undefined
    anotherViewer.modelManager.sceneState.setHoverId(eventData.objectId);
}

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ComponentsHoverChanged, onHoverChanged);

// 取消监听
viewer3D.removeEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ComponentsHoverChanged, onHoverChanged);
```