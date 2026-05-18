# 视频效果

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能添加视频
- 支持平面模式和投射模式两种展示方式
- 场景或构件状态变更后，投射模式下需调用 `video.update()`

## 平面模式

视频以独立平面的形式悬浮在场景中。

```javascript
const videoManagerConfig = new Glodon.Bimface.Plugins.Videos.VideoManagerConfig();
videoManagerConfig.viewer = viewer3D;
const videoManager = new Glodon.Bimface.Plugins.Videos.VideoManager(videoManagerConfig);

const videoConfig = new Glodon.Bimface.Plugins.Videos.VideoConfig();
videoConfig.viewer = viewer3D;
videoConfig.src = 'https://static.bimface.com/attach/xxx.mp4';
videoConfig.plane = { distance: 2000, side: 0 };
videoConfig.mute = false;
videoConfig.loop = false;

const video = new Glodon.Bimface.Plugins.Videos.Video(videoConfig);
videoManager.addVideo(video);
```

### 视频控制

```javascript
video.play();
video.pause();
video.mute(true);
video.loop(true);
```

## 投射模式

视频直接投射到模型表面。

```javascript
const videoConfig = new Glodon.Bimface.Plugins.Videos.VideoConfig();
videoConfig.viewer = viewer3D;
videoConfig.src = 'https://static.bimface.com/attach/xxx.mp4';
videoConfig.isPlaneOn = false;
videoConfig.camera = {
    position: { x: 4848, y: -15415, z: 661 },
    direction: { x: 1, y: 0, z: 0 },
    horizontalFov: Math.PI * 0.5,
    verticalFov: Math.PI * 0.344
};

const video = new Glodon.Bimface.Plugins.Videos.Video(videoConfig);
videoManager.addVideo(video);
```

### 更新投射位置

```javascript
// 当场景或构件状态变化后更新投射
video.update();
```
