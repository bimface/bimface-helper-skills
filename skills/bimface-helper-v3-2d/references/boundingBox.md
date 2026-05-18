# 包围盒（2D图纸）

## 使用约束与说明
- 2D图纸坐标系为二维，包围盒格式为 `{min: {x, y}, max: {x, y}}`（无z轴）
- 需要在图纸 `Loaded` 事件后才能获取或使用包围盒

## 获取场景包围盒

```javascript
// 获取当前图纸场景的包围盒
const sceneBox = viewer2D.getSceneBoundingBox();
// 返回格式: {"min": {"x": 0, "y": 0}, "max": {"x": 1000, "y": 1000}}
```

## 自定义包围盒定位

```javascript
// 定义包围盒并缩放定位
const boundingBox = {
    "min": { "x": 100, "y": 200 },
    "max": { "x": 500, "y": 600 }
};
viewer2D.zoomToBoundingBox(boundingBox, 0.25);
```

## 绘制包围盒效果

```javascript
// 在图纸上绘制自定义包围盒，外放系数默认为0，数值越大外放的距离越多
viewer2D.showBoundingBox(boundingBox, 1);
```

## 清除包围盒

```javascript
viewer2D.clearBoundingBox();
```
