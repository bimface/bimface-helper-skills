# 图层管理

## 使用约束与说明
- 必须在图纸 `Loaded` 事件触发后才能进行图层操作
- 图层数据属于单图纸，需通过 `viewer2D.getDrawing(modelId)` 获取图纸对象后操作
- 图层操作后需调用 `viewer2D.render()` 刷新视图

## 获取所有图层数据

```javascript
// 获取指定图纸的所有图层信息
const layersList = drawing2D.getLayers();
console.log(layersList);
// 每个图层包含 name 和 ID 等信息
```

## 隐藏指定图层

```javascript
viewer2D.hideLayer("3760153");
viewer2D.render();
```

## 显示指定图层

```javascript
viewer2D.showLayer("3760153");
viewer2D.render();
```

## 显示全部图层

```javascript
viewer2D.showAllLayers();
```
