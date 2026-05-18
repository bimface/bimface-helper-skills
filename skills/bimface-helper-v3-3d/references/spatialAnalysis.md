# 空间分析

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能创建分析工具
- 创建/更新分析后需调用 `viewer3D.render()` 刷新场景

## 可视域分析

在观察点位置，按视线方向和角度范围，分析可见区域与不可见区域。

```javascript
const viewshedManagerConfig = new Glodon.Bimface.Analysis.Viewshed.ViewshedManagerConfig();
viewshedManagerConfig.viewer = viewer3D;
const viewshedManager = new Glodon.Bimface.Analysis.Viewshed.ViewshedManager(viewshedManagerConfig);

const viewshedConfig = new Glodon.Bimface.Analysis.Viewshed.Viewshed3DConfig();
viewshedConfig.position = { x: 378009, y: -8583, z: 7546 };
viewshedConfig.direction = { x: -0.9, y: -0.10, z: 0.05 };
viewshedConfig.visibleAreaColor = new Glodon.Web.Graphics.Color(0, 255, 0, 0.6);
viewshedConfig.hiddenAreaColor = new Glodon.Web.Graphics.Color(255, 0, 0, 0.8);
viewshedConfig.distance = 60000;
viewshedConfig.horizontalFov = Math.PI / 4;
viewshedConfig.verticalFov = Math.PI / 8;

const viewshed = new Glodon.Bimface.Analysis.Viewshed.Viewshed3D(viewshedConfig);
viewshedManager.addViewshed(viewshed);
viewshedManager.update();
viewer3D.render();
```

## 控高分析

分析建筑是否超出指定高度限制。

```javascript
const heightLimitConfig = new Glodon.Bimface.Analysis.HeightLimit.HeightLimitAnalysisConfig();
heightLimitConfig.color = new Glodon.Web.Graphics.Color(255, 0, 0, 1);
heightLimitConfig.height = 80000;
heightLimitConfig.mode = 'customized';
heightLimitConfig.area = {
    type: 'circle',
    center: { x: 13742475, y: 26697538, z: 9512 },
    radius: 1200000
};
heightLimitConfig.viewer = viewer3D;

const heightLimit = new Glodon.Bimface.Analysis.HeightLimit.HeightLimitAnalysis(heightLimitConfig);
viewer3D.render();
```

### 切换分析模式

```javascript
// 切换到全局模式
heightLimit.setMode('global');
heightLimit.update();

// 切换到局部区域模式
heightLimit.setMode('customized');
heightLimit.update();
viewer3D.render();
```

## 透视分析

从观测点向多个目标点发出视线，分析通视情况。

```javascript
const sightlineConfig = new Glodon.Bimface.Analysis.Sightline.SightlineAnalysisConfig();
sightlineConfig.viewPoint = { x: 15046717, y: 27011164, z: 283704 };
sightlineConfig.targetPoints = [
    { x: 14061271, y: 25751289, z: 19513 },
    { x: 14187007, y: 26622248, z: 26 },
    { x: 14929600, y: 26350921, z: -999 }
];
sightlineConfig.visibleColor = new Glodon.Web.Graphics.Color(50, 211, 166, 1);
sightlineConfig.invisibleColor = new Glodon.Web.Graphics.Color(235, 0, 29, 1);
sightlineConfig.viewer = viewer3D;

const sightline = new Glodon.Bimface.Analysis.Sightline.SightlineAnalysis(sightlineConfig);
viewer3D.render();
```

### 清除透视分析

```javascript
sightline.destroy();
viewer3D.render();
```

## 天际线分析

绘制城市建筑物的天际轮廓线。

```javascript
const skylineConfig = new Glodon.Bimface.Analysis.Skyline.SkylineAnalysisConfig();
skylineConfig.style = {
    color: new Glodon.Web.Graphics.Color(252, 0, 26, 1.0),
    width: 2
};
skylineConfig.viewer = viewer3D;

const skyline = new Glodon.Bimface.Analysis.Skyline.SkylineAnalysis(skylineConfig);
viewer3D.render();
```

### 修改样式

```javascript
skyline.setStyle({
    color: new Glodon.Web.Graphics.Color(255, 246, 35, 1.0),
    width: 3
});
skyline.update();
viewer3D.render();
```

### 清除天际线

```javascript
skyline.destroy();
viewer3D.render();
```

## 射线检测

从指定起点沿方向发射射线，检测与构件的碰撞点。

```javascript
const conditions = [
    { categoryId: '-2000014' },
    { categoryId: '-2000023' }
];

const results = viewer3D.getComponentsByRaycaster(
    { x: 100, y: 200, z: 300 },
    { x: 0, y: -1, z: 0 },
    conditions
);

const hitPoint = results[0].position;
const distance = results[0].distance;
```

### 返回值结构

| 字段 | 类型 | 说明 |
|------|------|------|
| `position` | Object | 射线与构件的交点坐标 |
| `distance` | Number | 起点到交点的距离 |
| `layerId` | String | 交点所属图层ID |
| `objectId` | String | 命中构件ID |
