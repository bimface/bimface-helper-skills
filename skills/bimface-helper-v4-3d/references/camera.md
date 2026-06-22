# 相机状态与视角

## 使用约束与说明

- 相机对象通过 `viewer3D.getCamera()` 同步获取
- v4 中部分相机方法改为返回 Promise：`setStatus`、`setStandardView`、`zoomToBoundingBox`
- `setInteraction` 统一替代旧版分散的交互属性设置（`enablePan`、`enableZoom`、`enableTransitionEffect`、`rotateSensitivity`、`zoomSpeed`）
- `setType` 替代旧版 `setCameraType`
- `setStandardView` 替代旧版 `setView`
- 全局变量名统一使用：`viewer3D`、`app`、`model`、`camera`

---

## 获取相机

```javascript
var camera = viewer3D.getCamera();
```

---

## 相机类型：setType

设置相机投影类型：

```javascript
// 透视投影
camera.setType("Perspective");

// 正交投影
camera.setType("Orthographic");
```

---

## 相机交互行为：setInteraction

统一设置平移、缩放、过渡动画和旋转灵敏度等交互属性（替代旧版分散的 `enablePan`、`enableZoom`、`enableTransitionEffect`、`setRotateSensitivity`、`zoomSpeed`）：

```javascript
camera.setInteraction({
    basic: {
        pan: true,   // 启用平移
        zoom: true   // 启用缩放
    },
    advanced: {
        transition: true,           // 启用过渡动画
        rotateSensitivity: 1.0,     // 旋转灵敏度，默认 1.0
        zoomSpeed: 1.0              // 缩放速度，默认 1.0
    }
});
```

所有字段均为可选，传入部分字段时其余保持默认值：

```javascript
// 仅禁用旋转时的过渡动画
camera.setInteraction({
    advanced: {
        transition: false
    }
});
```

---

## 获取相机状态：getStatus

同步返回当前相机状态：

```javascript
var status = camera.getStatus();
// status 结构: { position: {x, y, z}, target: {x, y, z}, up: {x, y, z}, ... }
console.log("相机位置:", status.position);
console.log("视点目标:", status.target);
```

---

## 设置相机状态：setStatus

设置相机状态，返回 Promise（无回调参数）：

```javascript
var status = camera.getStatus();

// 修改位置后重新设置
status.position = { x: 100, y: 50, z: 200 };

camera.setStatus(status).then(function() {
    console.log("相机状态已更新");
});
```

> **注意**：v4 中 `setStatus` 不接收 callback 参数，结果必须通过 `.then()` 处理。

---

## 标准视角：setStandardView

切换到标准预定义视角，返回 Promise（替代旧版 `setView`）：

```javascript
// 可用枚举值
// Glodon.Bimface.Camera.ViewOption3D.Home      - 默认视角（前上45°）
// Glodon.Bimface.Camera.ViewOption3D.Front     - 前视图
// Glodon.Bimface.Camera.ViewOption3D.Back      - 后视图
// Glodon.Bimface.Camera.ViewOption3D.Left      - 左视图
// Glodon.Bimface.Camera.ViewOption3D.Right     - 右视图
// Glodon.Bimface.Camera.ViewOption3D.Top       - 俯视图
// Glodon.Bimface.Camera.ViewOption3D.Bottom    - 仰视图

camera.setStandardView(Glodon.Bimface.Camera.ViewOption3D.Home).then(function() {
    console.log("视角已切换到 Home");
});
```

---

## 视点缩放：zoomToBoundingBox

将相机视野缩放到指定包围盒，返回 Promise：

```javascript
// 参数说明
// - boundingBox: 包围盒 {max: {x, y, z}, min: {x, y, z}}
// - margin: 包围盒边距比例，默认 0.1（10%）
// - duration: 动画持续时间（毫秒），默认 1000
// - direction: 相机方向向量，默认 {x: 0, y: 0, z: -1}

camera.zoomToBoundingBox({
    boundingBox: {
        min: { x: -10, y: 0, z: -10 },
        max: { x: 10, y: 20, z: 10 }
    },
    margin: 0.1,
    duration: 1000,
    direction: { x: 0, y: 0, z: -1 }
}).then(function() {
    console.log("缩放到包围盒完成");
});
```

常用场景 —— 获取构件包围盒后缩放：

```javascript
// 获取构件包围盒（返回 Promise）
model.getBoundingBoxByIds(["componentId1", "componentId2"]).then(function(box) {
    // 缩放到构件包围盒
    camera.zoomToBoundingBox({
        boundingBox: box,
        margin: 0.05,
        duration: 800
    });
});
```

---

## 缩放到选中构件：zoomToSelection

将相机视野缩放到当前选中的构件，返回 Promise（**不接收 boundingBox 参数**，基于当前选中集）：

```javascript
// margin: 边距比例，默认 0.5
// margin > 0: 视野放大（包含更多周边），margin < 0: 视野缩小（更贴近构件）

// 缩放到当前选中构件
camera.zoomToSelection({
    margin: 0.5
}).then(function() {
    console.log("已缩放到选中构件");
});
```

常用场景 —— 选中构件后缩放：

```javascript
// 先选中构件
model.selectComponentsByIds(["componentId1", "componentId2"]);

// 再缩放到选中构件
camera.zoomToSelection({ margin: 0.3 }).then(function() {
    console.log("缩放完成");
});

// 或使用 async/await
await model.selectComponentsByIds(["componentId1"]);
await camera.zoomToSelection({ margin: 0.2 });
```

> **注意**：`zoomToSelection` 不接收 `boundingBox` 参数，它始终基于当前选中的构件集合进行缩放。如果当前没有选中任何构件，调用此方法可能无效果。

---

## 完整示例

```javascript
// 获取相机
var camera = viewer3D.getCamera();

// 配置交互行为
camera.setInteraction({
    basic: { pan: true, zoom: true },
    advanced: { transition: true, rotateSensitivity: 1.0, zoomSpeed: 1.0 }
});

// 切换到默认 Home 视角
camera.setStandardView(Glodon.Bimface.Camera.ViewOption3D.Home).then(function() {
    console.log("已切换到 Home 视角");
});

// 保存当前相机状态
var currentStatus = camera.getStatus();

// 恢复相机状态
camera.setStatus(currentStatus).then(function() {
    console.log("相机状态已恢复");
});

// 缩放到指定包围盒
camera.zoomToBoundingBox({
    boundingBox: { min: {x: 0, y: 0, z: 0}, max: {x: 50, y: 30, z: 20} },
    margin: 0.1,
    duration: 1500
}).then(function() {
    console.log("缩放完成");
});
```
