# 路径漫游（Walkthrough）

## 使用约束与说明

- 路径漫游有两套 API：**CameraWalkthrough**（自定义路径控制）和 **WalkthroughPanel**（UI 面板管理）
- CameraWalkthrough 由 `Plugin.Walkthrough.CameraWalkthrough` 构造函数创建
- WalkthroughPanel 通过 Widget 模式：`app.initializeWidget("WalkthroughPanel")` → `app.getWidget("WalkthroughPanel")`
- 命名空间为 `Plugin`（单数）
- 关键帧坐标必须使用世界坐标系（`coordinateSystem: 'world'`）
- 必须在模型加载完成后使用

---

## CameraWalkthrough —— 自定义路径

### 创建实例

```javascript
const cameraWalkthrough = new Glodon.Bimface.Plugin.Walkthrough.CameraWalkthrough({
  viewer: viewer3D
});
```

### addKeyFrame —— 添加关键帧

将当前相机位置作为一个关键帧添加到路径中：

```javascript
// 先将相机调整到期望位置
camera.setStatus({
  position: { x: 100, y: 50, z: 200 },
  target: { x: 0, y: 10, z: 0 },
  up: { x: 0, y: 0, z: 1 }
}).then(() => {
  // 添加当前相机位置为关键帧
  cameraWalkthrough.addKeyFrame();
});
```

### getKeyFrames —— 获取所有关键帧

```javascript
const keyFrames = cameraWalkthrough.getKeyFrames();
console.log("关键帧列表:", keyFrames);
```

### setKeyFrames —— 编程方式设置关键帧

```javascript
// 直接设置关键帧数组，无需手动调整相机
const keyFrameList = [
  {
    id: "kf_1",
    name: "入口",
    position: { x: 50, y: 10, z: 100 },
    target: { x: 0, y: 10, z: 0 },
    coordinateSystem: 'world'
  },
  {
    id: "kf_2",
    name: "大厅",
    position: { x: 0, y: 20, z: 50 },
    target: { x: 0, y: 10, z: 0 },
    coordinateSystem: 'world'
  },
  {
    id: "kf_3",
    name: "走廊",
    position: { x: -30, y: 15, z: 0 },
    target: { x: -50, y: 10, z: 0 },
    coordinateSystem: 'world'
  }
];

cameraWalkthrough.setKeyFrames(keyFrameList);
```

### setTime —— 设置漫游总时长

```javascript
// 设置漫游总时长为 30 秒
cameraWalkthrough.setTime({ totalTime: 30 });
```

### play —— 开始播放漫游

```javascript
cameraWalkthrough.play();
```

---

## WalkthroughPanel —— 漫游面板

### 初始化

```javascript
// 初始化 Widget
app.initializeWidget("WalkthroughPanel");
// 获取面板实例
const walkthroughPanel = app.getWidget("WalkthroughPanel");
```

### show

```javascript
walkthroughPanel.show();
```

### getWalkthroughManager —— 获取漫游管理器

```javascript
const walkthroughManager = walkthroughPanel.getWalkthroughManager();
```

#### addWalkthrough —— 添加漫游路径

```javascript
// 将 CameraWalkthrough 实例注册到面板中
walkthroughManager.addWalkthrough("建筑漫游路线", cameraWalkthrough);
```

### update —— 刷新面板

```javascript
walkthroughPanel.update();
```

---

## 完整示例

### 示例 1：添加关键帧方式（交互式添加）

```javascript
const viewer3D = new Glodon.Bimface.Viewer.Viewer3D(viewer3DConfig);
const app = viewer3D.getApp();
viewer3D.addModel(viewToken);
const camera = viewer3D.getCamera();

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelLoaded, () => {
  // 创建 CameraWalkthrough
  const cameraWalkthrough = new Glodon.Bimface.Plugin.Walkthrough.CameraWalkthrough({
    viewer: viewer3D
  });

  // 设置漫游时长
  cameraWalkthrough.setTime({ totalTime: 20 });

  // 初始化 WalkthroughPanel
  app.initializeWidget("WalkthroughPanel");
  const walkthroughPanel = app.getWidget("WalkthroughPanel");

  // 注册漫游路径
  walkthroughPanel.getWalkthroughManager().addWalkthrough("我的漫游", cameraWalkthrough);

  // 显示面板
  walkthroughPanel.show();

  // 用户手动调整视角后，点击按钮添加关键帧
  document.getElementById("btnAddKeyFrame").addEventListener("click", () => {
    cameraWalkthrough.addKeyFrame();
    walkthroughPanel.update();
    console.log("当前关键帧数:", cameraWalkthrough.getKeyFrames().length);
  });

  // 开始播放漫游
  document.getElementById("btnPlay").addEventListener("click", () => {
    cameraWalkthrough.play();
  });
});
```

### 示例 2：预设关键帧方式（编程定义）

```javascript
const viewer3D = new Glodon.Bimface.Viewer.Viewer3D(viewer3DConfig);
const app = viewer3D.getApp();
viewer3D.addModel(viewToken);

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelLoaded, () => {
  // 创建 CameraWalkthrough
  const cameraWalkthrough = new Glodon.Bimface.Plugin.Walkthrough.CameraWalkthrough({
    viewer: viewer3D
  });

  // 编程定义关键帧
  const keyFrames = [
    {
      id: "kf_1",
      name: "正门入口",
      position: { x: 80, y: 5, z: 120 },
      target: { x: 0, y: 8, z: 0 },
      coordinateSystem: 'world'
    },
    {
      id: "kf_2",
      name: "一楼大堂",
      position: { x: 0, y: 12, z: 60 },
      target: { x: 0, y: 8, z: 0 },
      coordinateSystem: 'world'
    },
    {
      id: "kf_3",
      name: "二楼走廊",
      position: { x: -20, y: 22, z: 30 },
      target: { x: -40, y: 18, z: 0 },
      coordinateSystem: 'world'
    },
    {
      id: "kf_4",
      name: "屋顶俯瞰",
      position: { x: 0, y: 50, z: 0 },
      target: { x: 0, y: 0, z: 0 },
      coordinateSystem: 'world'
    }
  ];

  cameraWalkthrough.setKeyFrames(keyFrames);

  // 设置漫游时长为 25 秒
  cameraWalkthrough.setTime({ totalTime: 25 });

  // 初始化面板并注册
  app.initializeWidget("WalkthroughPanel");
  const walkthroughPanel = app.getWidget("WalkthroughPanel");
  walkthroughPanel.getWalkthroughManager().addWalkthrough("预设路线", cameraWalkthrough);
  walkthroughPanel.show();

  // 自动开始播放
  setTimeout(() => {
    cameraWalkthrough.play();
  }, 1000);
});
```

---

## 注意事项

- 关键帧中的 `coordinateSystem` 必须设为 `'world'`
- `addKeyFrame()` 记录的是当前相机位置，因此需要先调整相机再调用
- `setKeyFrames()` 会覆盖之前的所有关键帧
- WalkthroughPanel 需要调用 `update()` 来刷新关键帧列表显示
