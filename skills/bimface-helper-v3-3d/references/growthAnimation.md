# 生长动画

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能创建生长动画
- 生长动画通过 `GrowthAnimationConfig` 配置参与动画的构件、时长和生长方向
- 播放/停止动画后无需调用 `viewer3D.render()`

## 播放生长动画

```javascript
// 构造生长动画配置项
let growthAnimationConfig = new Glodon.Bimface.Plugins.Animation.GrowthAnimationConfig();
growthAnimationConfig.viewer = viewer3D;

// 设置参与生长动画的构件
// conditions中需包含modelId和objectData，objectData为数组，可参考 [筛选构件](filterComponents.md)
growthAnimationConfig.conditions = [
  {
    modelId: '模型ID',
    objectData: [
      { categoryId: "-2000011", family: "基本墙" },
      { categoryId: "-2001330" },
      { levelName: "Roof" },
      { levelName: "Parapet" },
      { levelName: "03 - Floor", categoryId: "-2000038" },
      { categoryId: "-2000023" }
    ]
  }
];

// 设置生长动画的时长，单位为毫秒
growthAnimationConfig.time = 5000;

// 设置生长方向
growthAnimationConfig.direction = { x: 0, y: 0, z: 1 };

// 构造生长动画对象
let growthAnimation = new Glodon.Bimface.Plugins.Animation.GrowthAnimation(growthAnimationConfig);

// 播放生长动画
growthAnimation.play();
```

## 停止生长动画

```javascript
// 停止生长动画
growthAnimation.stop();
```

## 监听动画进度

```javascript
// 设置生长动画的进度监听
// progress取值范围为0-1，1表示动画播放完成
growthAnimation.onProgressChanged(function (progress) {
  if (progress == 1) {
    // 动画播放完成
    console.log("生长动画播放完成");
  }
});
```
