# 图元操作

## 使用约束与说明
- 必须在图纸 `Loaded` 事件触发后、`drawing2D` 可用时才能调用
- 图元操作通过 `viewer2D.getDrawing(modelId)` 获取的图纸对象执行，不传modelId则对默认图纸操作
- 操作后可能需调用 `viewer2D.render()` 刷新视图

## 选中图元

```javascript
// 选中指定图元ID列表
drawing2D.select({
    objectIds: ["5067665", "5067666"]
});

// 选中全部图元
drawing2D.select({ all: true });
```

## 清除选中

```javascript
// 清除所有图元选中状态
viewer2D.clearSelection();
```

## 获取已选中的图元

```javascript
// 获取选中集合中的图元ID数组
const selectedIds = viewer2D.getSelectedObjects();
```

## 高亮图元

```javascript
// 高亮指定图元ID列表
drawing2D.highlight({
    objectIds: ["5067665"]
});

// 高亮全部图元
drawing2D.highlight({ all: true });
```

## 清除高亮

```javascript
viewer2D.clearHighlight();
```

## 隐藏图元

```javascript
// 隐藏指定图元ID列表
drawing2D.hide({
    objectIds: ["5067665"]
});

// 隐藏全部图元
drawing2D.hide({ all: true });
```

## 显示图元

```javascript
// 显示指定图元ID列表
drawing2D.show({
    objectIds: ["5067665"]
});

// 显示全部图元
viewer2D.showAllElements();
```

## 设置图元透明度

```javascript
// 设置指定图纸的图元透明度，取值范围0~1
drawing2D.setOpacity(0.5);
```

## 定位到图元

```javascript
// 缩放并定位到指定图元
viewer2D.zoomToObject("5067665");
```

## 清除所有包围盒

```javascript
viewer2D.clearBoundingBox();
```
