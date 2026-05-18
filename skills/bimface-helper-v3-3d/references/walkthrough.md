# 路径漫游

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能创建路径漫游对象
- 需要先调整好视角，再调用 `addKeyFrame()` 记录关键帧，才能播放出预期的漫游路径
- 播放前至少需要2个关键帧
- `setKeyFrameCallback()` 和 `stopCallback()` 中的回调函数必须为具名函数（确保可注销）

## 创建路径漫游

```javascript
const config = new Glodon.Bimface.Plugins.Walkthrough.WalkthroughConfig();
config.viewer = viewer3D;
const wt = new Glodon.Bimface.Plugins.Walkthrough.Walkthrough(config);
```

## 通过录制添加关键帧

```javascript
// 调整视角后调用，逐个记录当前视角作为关键帧
wt.addKeyFrame();
```

## 通过数据对象批量设置关键帧

适用于提前规划好完整漫游路径的场景，无需逐个录制。

```javascript
const keyFrames = [
  {
    id: "21124876-8436-4a7e-8f3f-50f30876baeb",
    position: { x: 54338.306, y: 24823.562, z: 19009.247 },
    target: { x: 138966.225, y: -343576.473, z: 47971.612 },
    coordinateSystem: "world",
  },
  {
    id: "86f4dd8e-8a43-4d4d-8aa5-cf5e3cdf16a1",
    position: { x: 55392.233, y: 19177.558, z: 17950.450 },
    target: { x: 237925.634, y: -311824.030, z: 60086.961 },
    coordinateSystem: "world",
  }
];
wt.setKeyFrames(keyFrames);
```

## 获取关键帧列表

```javascript
const keyFrames = wt.getKeyFrames();
```

## 播放与暂停

```javascript
// 设置漫游总时长（秒）
wt.setWalkthroughTime(5);

// 从第一个关键帧开始播放
const list = wt.getKeyFrames();
wt.play(list[0].id);

// 暂停
wt.pause();

// 停止
wt.stop();
```

## 逐帧时间设置

可分别设置每个关键帧的停留时间和帧间过渡时间。

```javascript
wt.setWalkthroughTime({
    totalTime: 2,
    frameTime: [
        { id: "21124876-...", stayTime: 1, timeBetweenFrames: 2 },
        { id: "86f4dd8e-...", stayTime: 1, timeBetweenFrames: 2 },
        { id: "d1d3d7bb-...", stayTime: 2, timeBetweenFrames: 3 },
        { id: "69e3cae8-...", stayTime: 2, timeBetweenFrames: 2 },
        { id: "5475b7eb-...", stayTime: 2, timeBetweenFrames: null }
    ]
});
```

## 关键帧回调

到达每个关键帧时触发回调，可用于在特定视角执行构件着色、标签弹出等操作。播放结束触发 `stopCallback`。

```javascript
// 到达关键帧时触发，idx 为当前关键帧索引（从0开始）
wt.setKeyFrameCallback(onKeyFrameReached);

function onKeyFrameReached(idx) {
    console.log("Current key frame index is " + idx);
}

// 播放结束回调
wt.stopCallback(onWalkthroughStopped);

function onWalkthroughStopped() {
    drawableContainer.clear();
    viewer3D.enableBlinkComponents(false);
    viewer3D.restoreAllDefault();
}
```

## 路径漫游数据保存与加载

可序列化/反序列化漫游面板中的路径数据，用于跨会话持久化。

```javascript
// 保存路径漫游数据（获取面板上的漫游数据并序列化）
app3D.getWalkthroughData(function (data) {
    const walkthroughData = JSON.stringify(data);
    console.log(walkthroughData);
});

// 加载路径漫游数据（反序列化后初始化到面板上）
const walkthroughList = JSON.parse(walkthroughData);
app3D.initializeWalkthroughData(walkthroughList);
```

### 监听漫游面板状态

```javascript
app3D.addEventListener(
    Glodon.Bimface.Application.WebApplication3DEvent.WalkthroughStateChanged,
    function (walkPlaneState) {
        // walkPlaneState 为 'On' 或 'Off'
    }
);
```

## 第三人称漫游

以第三人称视角方式在场景中自由漫游，可调节飞行速度。

```javascript
// 开启第三人称漫游模式
viewer3D.setNavigationMode(Glodon.Bimface.Viewer.NavigationMode3D.ThirdPerson);

// 设置飞行速度倍率（默认1）
viewer3D.setFlySpeedRate(8);
```
