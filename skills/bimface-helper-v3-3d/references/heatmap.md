# 热力图

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能创建热力图
- 数据点格式为 `{ x, y, z, value }`
- 修改配色后需调用 `update()`

## 二维热力图

```javascript
const boundary = [
    { x: -1000, y: -1000, z: 0 },
    { x: 1000, y: -1000, z: 0 },
    { x: 1000, y: 1000, z: 0 },
    { x: -1000, y: 1000, z: 0 }
];

const heatMap2DConfig = new Glodon.Bimface.Plugins.Heatmap.Heatmap2DConfig();
heatMap2DConfig.enableColorLegend = true;
heatMap2DConfig.viewer = viewer3D;
heatMap2DConfig.boundary = boundary;

const heatMap2D = new Glodon.Bimface.Plugins.Heatmap.Heatmap2D(heatMap2DConfig);

const heatData = [
    { x: 0, y: 0, z: 0, value: 80 },
    { x: 500, y: 300, z: 0, value: 45 }
];
heatMap2D.setData(heatData);
heatMap2D.setRadius(80);
heatMap2D.update();
heatMap2D.show();
viewer3D.render();
```

## 三维热力图

```javascript
const heatMap3DConfig = new Glodon.Bimface.Plugins.Heatmap.Heatmap3DConfig();
heatMap3DConfig.enableColorLegend = true;
heatMap3DConfig.viewer = viewer3D;
heatMap3DConfig.radius = 200;
heatMap3DConfig.boundary = boundary;
heatMap3DConfig.boundaryHeight = 3000;

const heatMap3D = new Glodon.Bimface.Plugins.Heatmap.Heatmap3D(heatMap3DConfig);
heatMap3D.setData(heatData);
heatMap3D.update();
heatMap3D.show();
viewer3D.render();
```

## 自定义配色

```javascript
const colorMap = {
    0.0: new Glodon.Web.Graphics.Color(0, 0, 255, 1),
    0.5: new Glodon.Web.Graphics.Color(0, 255, 0, 1),
    1.0: new Glodon.Web.Graphics.Color(255, 0, 0, 1)
};
heatMap2D.setHeatMapColor(colorMap);
heatMap2D.update();
```

## 销毁热力图

```javascript
heatMap2D.dispose();
viewer3D.render();
```
