# 曲线效果

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能初始化曲线和外部构件管理器
- `setMap` 是异步回调，贴图加载完成后的逻辑需在回调函数中处理
- `extObjMng.loadObject` 是异步的，外部构件操作需在回调中执行

## 构造曲线

曲线动态效果需要按照以下步骤来创建
1）创建外部构件管理器，如果已经创建了可以复用
2）创建对应的曲线
3）设置曲线贴图，并通过外部构件管理器引入至场景内
4）创建动画并播放

### 创建外部构件管理器

```javascript
// 创建外部构件管理器
const extObjMng = new Glodon.Bimface.Plugins.ExternalObject.ExternalObjectManager(viewer3D);
```

### 构造曲线

```javascript
// 曲线配置
const color = new Glodon.Web.Graphics.Color(255, 52, 0, 1.0);
const width = 2;
const style = {
    "lineType": "Continuous",
    "lineStyle": null
};

// 曲线坐标点数组
const points = [
    { x: 14425257, y: 24064823, z: 0 },
    { x: 14294726, y: 24632057, z: 0 },
    { x: 14122681, y: 25016419, z: 0 },
    // ... 更多坐标点
];

// 构造曲线
const splineCurve = new Glodon.Bimface.Plugins.Geometry.SplineCurve({
    points: points,
    viewer: viewer3D,
    color: color,
    width: width,
    style: style
});
```

### 构造曲线（Setter方式）

亦可使用 `setXxx` 方法逐步配置曲线属性，并设置曲线类型。

```javascript
// 使用坐标点数组构造曲线
let splineCurve = new Glodon.Bimface.Plugins.Geometry.SplineCurve(points);

// 设置线宽
splineCurve.setWidth(4);

// 设置颜色
splineCurve.setColor(new Glodon.Web.Graphics.Color(255, 0, 0, 1));

// 设置线型
splineCurve.setStyle({
  "lineType": "Continuous",  // Continuous: 实线, Dashed: 虚线
  "lineStyle": null
});

// 虚线的线型配置示例
splineCurve.setStyle({
  "lineType": "Dashed",
  "lineStyle": {
    "dashLength": 70000,    // 虚线中单个短划线的长度
    "gapLength": 40000       // 虚线中短划线之间间隙的长度
  }
});

// 设置曲线类型: "spline"（样条曲线）或 "polyline"（折线）
splineCurve.setType("polyline");
```

### 拉伸曲线

将二维曲线拉伸为三维效果，通常用于飞线等场景。

```javascript
splineCurve.stretch();
```

## 设置曲线贴图

```javascript
// 设置曲线贴图
splineCurve.setMap({
    src: "https://example.com/stream.png",  // 贴图图片URL
    enableColorOverride: false  // 是否允许颜色覆盖
}, function() {
    // 贴图加载完成后的回调
    const objectName = 'splineCurve';
    extObjMng.loadObject({ name: objectName, object: splineCurve });
    const curveId = extObjMng.getObjectIdByName(objectName);
});
```

## 曲线动画

### 创建曲线动画

```javascript
// 构造曲线动画的配置项
const curveAnimationConfig = new Glodon.Bimface.Plugins.Animation.CurveAnimationConfig();

// 配置Viewer对象
curveAnimationConfig.viewer = viewer3D;

// 配置曲线对象（数组）
curveAnimationConfig.curves = [splineCurve];

// 配置动画时间（毫秒），与speed二选一
curveAnimationConfig.time = 1500;

// 或配置动画速度（单位/毫秒），与time二选一
curveAnimationConfig.speed = 12000000;

// 配置是否循环播放
curveAnimationConfig.loop = true;

// 配置动画类型
curveAnimationConfig.type = "flow";

// 构造曲线动画对象
const curveAnimation = new Glodon.Bimface.Plugins.Animation.CurveAnimation(curveAnimationConfig);
```

### 播放动画

```javascript
// 播放动画
curveAnimation.play();
```

### 停止动画

```javascript
// 停止动画
curveAnimation.stop();
```

## 辉光效果

### 添加辉光效果

```javascript
// 辉光效果配置
const bloomEffectConfig = new Glodon.Bimface.Plugins.Effect.BloomEffectConfig();

// 设置构件ID数组
bloomEffectConfig.ids = [{ "objectIds": curveIds }];

// 设置辉光强度
bloomEffectConfig.intensity = 0.8;

// 设置扩散程度
bloomEffectConfig.spread = 3;

// 关联viewer
bloomEffectConfig.viewer = viewer3D;

// 构造辉光效果
const bloomEffect = new Glodon.Bimface.Plugins.Effect.BloomEffect(bloomEffectConfig);
```
