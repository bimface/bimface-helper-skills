# 监听事件

## 使用约束与说明

- 使用 `addEventListener` / `removeEventListener` 模式
- 监听函数必须使用**命名函数**，不可使用匿名函数（否则无法正确移除监听）
- 所有事件常量位于 `Glodon.Bimface.Viewer.Viewer3DEvent` 命名空间下
- 部分事件返回的 `eventData` 可能包含 `undefined` 值，需做防御性检查
- 全局变量名统一使用：`viewer3D`、`app`、`model`、`camera`

---

## 事件清单

### ViewAdded — 视图加载完成

在视图（View）添加/加载完成时触发，此时可以安全地获取 model 对象：

```javascript
function onViewAdded(eventData) {
    model = viewer3D.getModel();
    camera = viewer3D.getCamera();
    // 执行依赖于模型和相机的后续操作
}

// 添加监听
viewer3D.addEventListener(
    Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
    onViewAdded
);

// 移除监听
viewer3D.removeEventListener(
    Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
    onViewAdded
);
```

---

### ModelAdded — 模型添加完成

在模型数据添加完成时触发，`eventData` 为 `modelId` 字符串，可用于获取指定模型：

```javascript
function onModelAdded(modelId) {
    var addedModel = viewer3D.getModel(modelId);
    // 对新增的模型进行操作
}

viewer3D.addEventListener(
    Glodon.Bimface.Viewer.Viewer3DEvent.ModelAdded,
    onModelAdded
);
```

---

### MouseClicked — 鼠标点击

返回事件数据对象，包含点击类型、构件信息和空间坐标：

- `eventType`: `"Click"`（左键）或 `"RightClick"`（右键）
- `modelId`: 被点击构件所属模型 ID（可能为 `undefined`）
- `objectId`: 被点击构件 ID（点击空白区域时为 `undefined`）
- `worldPosition`: 世界坐标 `{x, y, z}`
- `normal`: 法线方向 `{x, y, z}`
- `boundingBox`: 构件的包围盒

```javascript
function onMouseClicked(eventData) {
    // 防御检查：未点击到构件时直接返回
    if (!eventData.objectId) return;

    if (eventData.eventType === "Click") {
        // 左键点击：选中构件
        model.addSelection({ids: [eventData.objectId]});
    }

    console.log("点击位置:", eventData.worldPosition);
}

viewer3D.addEventListener(
    Glodon.Bimface.Viewer.Viewer3DEvent.MouseClicked,
    onMouseClicked
);
```

---

### MouseDoubleClicked — 鼠标双击

返回事件数据对象，`eventType` 固定为 `"DoubleClick"`：

- `eventType`: `"DoubleClick"`
- `modelId`: 被双击构件所属模型 ID（可能为 `undefined`）
- `objectId`: 被双击构件 ID
- `worldPosition`: 世界坐标 `{x, y, z}`
- `normal`: 法线方向 `{x, y, z}`

```javascript
function onMouseDoubleClicked(eventData) {
    if (!eventData.objectId) return;

    // 双击缩放到构件
    camera.zoomToBoundingBox({
        boundingBox: eventData.boundingBox,
        duration: 1000
    });
}

viewer3D.addEventListener(
    Glodon.Bimface.Viewer.Viewer3DEvent.MouseDoubleClicked,
    onMouseDoubleClicked
);
```

---

### SelectionChangedInModel — 选中状态变更

选中构件发生变化时触发，`eventData` 结构为 `{ modelId: [componentIds] }`：

```javascript
function onSelectionChanged(eventData) {
    // eventData 结构: { "模型ID": ["构件ID1", "构件ID2", ...] }
    for (var modelId in eventData) {
        if (eventData.hasOwnProperty(modelId)) {
            var selectedIds = eventData[modelId];
            console.log("模型 " + modelId + " 的选中构件:", selectedIds);

            // 示例：遍历并隐藏选中的构件
            var targetModel = viewer3D.getModel(modelId);
            selectedIds.forEach(function(componentId) {
                targetModel.hideComponents({ids: [componentId]});
            });
        }
    }
}

viewer3D.addEventListener(
    Glodon.Bimface.Viewer.Viewer3DEvent.SelectionChangedInModel,
    onSelectionChanged
);
```

---

### Rendered — 每帧渲染完成

在该帧渲染完成时触发，适合执行每帧更新的逻辑：

```javascript
function onRendered() {
    // 不执行耗时操作，避免阻塞渲染循环
    updateCustomOverlay();
}

viewer3D.addEventListener(
    Glodon.Bimface.Viewer.Viewer3DEvent.Rendered,
    onRendered
);
```

---

### ComponentsHoverChanged — 鼠标悬停构件变更

鼠标悬停的构件发生变化时触发，返回 `{ objectId }`（移出所有构件时 `objectId` 为 `undefined`）：

```javascript
function onComponentsHoverChanged(eventData) {
    if (eventData.objectId) {
        // 悬停在构件上时可通过着色高亮
        var highlightColor = new Glodon.Bimface.Common.Graphics.Color(255, 255, 0, 0.5);
        model.overrideComponentColor({ids: [eventData.objectId]}, highlightColor);
    } else {
        // 移出构件时恢复原始颜色
        model.restoreComponentColor({all: true});
    }
}

viewer3D.addEventListener(
    Glodon.Bimface.Viewer.Viewer3DEvent.ComponentsHoverChanged,
    onComponentsHoverChanged
);
```

---

## 完整示例：注册与清理

```javascript
var viewer3D, app, model, camera;

// 定义所有监听函数（命名函数）
function onViewAdded() {
    model = viewer3D.getModel();
    camera = viewer3D.getCamera();
}

function onMouseClicked(eventData) {
    if (!eventData.objectId) return;
    console.log("点击了构件:", eventData.objectId);
}

function onComponentsHoverChanged(eventData) {
    if (eventData.objectId) {
        var highlightColor = new Glodon.Bimface.Common.Graphics.Color(255, 255, 0, 0.5);
        model.overrideComponentColor({ids: [eventData.objectId]}, highlightColor);
    } else {
        model.restoreComponentColor({all: true});
    }
}

// 注册事件监听
function registerEvents() {
    viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded, onViewAdded);
    viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.MouseClicked, onMouseClicked);
    viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ComponentsHoverChanged, onComponentsHoverChanged);
}

// 移除事件监听
function unregisterEvents() {
    viewer3D.removeEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded, onViewAdded);
    viewer3D.removeEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.MouseClicked, onMouseClicked);
    viewer3D.removeEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ComponentsHoverChanged, onComponentsHoverChanged);
}
```
