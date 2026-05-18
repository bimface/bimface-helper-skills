# 视图控制

## 使用约束与说明
- 必须在图纸 `Loaded` 事件触发后才能进行视图控制操作
- 视图交互控制可设置是否允许拖拽、缩放等

## Home视角

```javascript
// 缩放视图比例以显示当前视口内的所有图元
viewer2D.home();
```

## 框选放大

```javascript
// 进入框选放大模式，操作完成后自动退出该模式
viewer2D.rectZoom();
```

## 获取缩放比例

```javascript
// 获取当前的缩放比例
const zoomFactor = viewer2D.getZoomFactor();

// 获取滚轮缩放的倍率
const zoomRatio = viewer2D.getZoomRatio();
```

## 视图交互开关

```javascript
// 设置是否允许视图场景拖动，默认为true
viewer2D.enableDrag(true);
viewer2D.enableDrag(false);

// 设置是否允许视图场景缩放，默认为true
viewer2D.enableScale(true);
viewer2D.enableScale(false);
```

## 全屏显示

```javascript
// 进入全屏显示
viewer2D.enableFullScreen(true);

// 退出全屏显示
viewer2D.enableFullScreen(false);
```

## 鼠标悬停效果

```javascript
viewer2D.enableHover(true);
viewer2D.enableHover(false);
```

## 选中效果开关

```javascript
// 是否开启选中效果，默认为true
viewer2D.enablePickEffect(true);
```

## 框选图元开关

```javascript
// 是否允许框选图元，默认为true
viewer2D.enableCrossingSelection(true);

// 结束框选模式
viewer2D.endBoxSelection();

// 获取框选模式: Default / Window / Crossing
const boxMode = viewer2D.getBoxSelectionMode();
```

## 坐标转换

```javascript
// 获取客户端坐标对应的世界坐标
const worldPos = viewer2D.clientToWorld({ x: 300, y: 200 });
// 返回 Glodon.Web.Geometry.Point3d 对象
```

## 小地图

```javascript
// 显示图纸小地图
viewer2D.enableMiniMap(true, function() {
    console.log("小地图加载完成");
});

// 关闭小地图
viewer2D.enableMiniMap(false);
```
