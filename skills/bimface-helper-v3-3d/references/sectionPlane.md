# 剖切面

## 使用约束与说明
- 剖切面的初始化必须在场景加载完成（触发 `ViewAdded` 事件）之后进行
- 一个场景内可以创建多个剖切面实例，但同一时间仅一个剖切面生效
- 所有改变剖切面视觉状态的操作后，必须调用 `viewer3D.render()` 刷新场景才能生效
- 剖切面与剖切盒（SectionBox）互斥，同一场景不可同时使用

## 创建剖切面

```JavaScript
// 声明为模块级或全局变量，供后续显隐控制使用
let sectionPlane;

function initSectionPlane() {
  // 防御性设计：避免重复创建
  if (sectionPlane) {
    return;
  }

  // 剖切面配置类
  const config = new Glodon.Bimface.Plugins.Section.SectionPlaneConfig();
  // 关联全局场景查看器（必填）
  config.viewer = viewer3D;
  // 剖切面类型，可选 X / Y / Z，默认为 X（垂直于 X 轴）
  config.plane = Glodon.Bimface.Plugins.Section.SectionPlanePlane.X;
  // 剖切面方向，默认为 Forward
  config.direction = Glodon.Bimface.Plugins.Section.SectionPlaneDirection.Forward;
  // 剖切进度 [0,100]，默认为 50
  config.progress = 50;
  // 是否开启补面填充，默认为 true
  config.isHatchEnabled = true;
  // 剖切的筛选条件（可选），筛选条件格式见 [构件筛选](references/filter.md)
  // config.filter = [{ modelId: "模型ID", objectIds: ["构件ID1", "构件ID2"] }];

  // 初始化剖切面对象
  sectionPlane = new Glodon.Bimface.Plugins.Section.SectionPlane(config);
  console.log("剖切面初始化完成");
}
```

## 剖切面的显隐与重置

```JavaScript
// 确保已初始化
if (!sectionPlane) {
  initSectionPlane();
}

// 显示剖切面
sectionPlane.showPlane();
viewer3D.render();

// 隐藏剖切面
sectionPlane.hidePlane();

// 重置剖切面
sectionPlane.reset();
viewer3D.render();
```

## 设置和获取剖切方向

```JavaScript
if (sectionPlane) {
  // 设置剖切方向为 Reverse（反向）
  sectionPlane.setDirection(Glodon.Bimface.Plugins.Section.SectionPlaneDirection.Reverse);
  viewer3D.render();

  // 获取当前剖切方向
  const currentDirection = sectionPlane.getDirection();
}
```

## 设置和获取剖切进度

```JavaScript
if (sectionPlane) {
  // 设置剖切进度为 80（范围 0~100）
  sectionPlane.setProgress(80);
  viewer3D.render();

  // 获取当前剖切进度
  const currentProgress = sectionPlane.getProgress();
}
```

## 自定义剖切面位置

```JavaScript
if (sectionPlane) {
  // 通过原点和方向向量定义任意剖切面
  // origin: 剖切平面上的一个点 {x, y, z}
  // direction: 平面法线方向向量 {x, y, z}
  // offset: 沿法线方向的偏移量
  const origin = { x: 0, y: 0, z: 0 };
  const direction = { x: 0, y: 0, z: 1 };
  const offset = 0;

  sectionPlane.setPositionByPlane(origin, direction, offset);
  viewer3D.render();
}
```

## 获取和设置剖切面状态

```JavaScript
let savedPlaneState;
// 1. 获取并保存当前剖切状态
if (sectionPlane) {
  savedPlaneState = sectionPlane.getState();
  console.log("剖切面状态已保存", savedPlaneState);
}

// 2. 恢复此前保存的剖切状态
if (sectionPlane && savedPlaneState) {
  sectionPlane.setState(savedPlaneState);
  viewer3D.render();
}
```


