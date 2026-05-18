# ViewerDrawing中的监听事件

## 使用约束与说明
- 具名函数原则：事件处理函数必须是一个具名函数，不能是匿名函数，否则注册的事件无法被注销
- 严格按照下方文档说明的数据结构获取事件参数，不要捏造不存在的属性
- 代码块中的eventHandler是一个示例函数，不意味着每个事件的处理函数都是一样的函数名称或内容

## 添加和注销事件监听器（基础范式）
```javascript
// 定义事件处理函数
const eventHandler = function (eventData) {
    console.log("事件触发", eventData);
};

// 添加事件监听器
viewer2D.addEventListener('eventName', eventHandler);

// 注销事件监听器
viewer2D.removeEventListener('eventName', eventHandler);
```

## 图纸加载完成事件
```javascript
viewer2D.addEventListener(Glodon.Bimface.Viewer.ViewerDrawingEvent.Loaded, eventHandler);
```

## 图纸渲染完成事件
```javascript
viewer2D.addEventListener(Glodon.Bimface.Viewer.ViewerDrawingEvent.Rendered, eventHandler);
```

## 视图变化事件
```javascript
// 视图正在移动
viewer2D.addEventListener(Glodon.Bimface.Viewer.ViewerDrawingEvent.ViewMoving, eventHandler);

// 视图移动完毕
viewer2D.addEventListener(Glodon.Bimface.Viewer.ViewerDrawingEvent.ViewMoved, eventHandler);

// 视图正在缩放
viewer2D.addEventListener(Glodon.Bimface.Viewer.ViewerDrawingEvent.ViewZooming, eventHandler);

// 视图缩放完毕
viewer2D.addEventListener(Glodon.Bimface.Viewer.ViewerDrawingEvent.ViewZoomed, eventHandler);

// 视图发生变化（综合事件）
viewer2D.addEventListener(Glodon.Bimface.Viewer.ViewerDrawingEvent.ViewChanged, eventHandler);
```

## 鼠标点击事件
```javascript
const eventHandler = function (eventData) {
    // 防御性判断：如果没有点中图元，则不执行操作
    if (!eventData.objectId) {
        return;
    }
    console.log("点击事件触发", eventData);
};

viewer2D.addEventListener(Glodon.Bimface.Viewer.ViewerDrawingEvent.MouseClicked, eventHandler);
```
点击事件触发后，会返回一个对象，包含以下属性：
- `eventType`：`"Click"`（左键点击）或 `"RightClick"`（右键点击）
- `objectId`：点击的图元ID，仅点击在图元上时返回
- `worldPosition`：点击位置的世界坐标，格式为`{x: 100, y: 200}`

## 图元选中变化事件
```javascript
const eventHandler = function (eventData) {
    // eventData 为选中图元的ID数组
    console.log("选中的图元ID:", eventData);
};

viewer2D.addEventListener(Glodon.Bimface.Viewer.ViewerDrawingEvent.ComponentsSelectionChanged, eventHandler);
```

## 图纸测量事件
```javascript
viewer2D.addEventListener(Glodon.Bimface.Viewer.ViewerDrawingEvent.DrawingMeasure, eventHandler);
```

## 错误事件
```javascript
const eventHandler = function (error) {
    console.error("图纸加载出错:", error);
};

viewer2D.addEventListener(Glodon.Bimface.Viewer.ViewerDrawingEvent.Error, eventHandler);
```
