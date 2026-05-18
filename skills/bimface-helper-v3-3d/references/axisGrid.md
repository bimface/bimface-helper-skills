# 轴网

## 使用约束与说明
- 必须在`ViewAdded`事件触发后、`model3D`可用时才能调用
- `floorId` 可通过 `model3D.getFloors()` 异步获取（在回调中处理结果）
- 事件监听器必须使用具名函数（参照 [监听事件](events.md) 规范）

## 显示单层轴网

```javascript
model3D.getFloors(function (floors) {
  foreach(floors, function (floor) {
    // fileId: 文件ID，单模型为空字符串，集成模型传对应子文件ID；floorId: 楼层ID
    model3D.showAxisGridsByFloor(fileId, floor.id);
  });
  // 设置轴号一直显示在窗口内，相机范围外的轴号将显示在窗口边缘
  model3D.enableShowGridBubblesAlongWithAxis(true);
});

```

## 隐藏单层轴网

```javascript
model3D.removeAxisGridsByFloor(fileId, floorId);
```

## 轴网透视效果

开启透视效果后，轴网就不会被其他构件遮挡
```javascript
// 开启轴网透视效果
model3D.bringAxisGridsToFront(true);

// 取消轴网透视效果
model3D.bringAxisGridsToFront(false);
```

## 轴网Hover效果

```javascript
// 激活轴网Hover
viewer3D.enableAxisGridsHover(true);

// 取消轴网Hover
viewer3D.enableAxisGridsHover(false);
```

## Hover显示标签事件

```javascript
// 添加Hover显示标签事件
viewer3D.addEventListener("Hover", function(data) {
    // data.worldPosition: Hover位置的坐标
    // data.objectId: Hover的构件ID
});

// 移除Hover显示标签事件
viewer3D.removeEventListener("Hover", callback);
```

## 轴网状态管理

```javascript
// 获取轴网状态
const state = model3D.getCurrentAxisGridsState();

// 恢复轴网状态
model3D.setAxisGridsState(state);
```

## 轴网颜色设置

```javascript
// 替换轴号颜色
const color = new Glodon.Web.Graphics.Color(255, 0, 0, 0.5);
model3D.setGridBubblesColor(color);

// 替换轴线颜色
const color = new Glodon.Web.Graphics.Color(0, 0, 255, 0.5);
model3D.setGridLinesColor(color);
```

## 获取最近轴网

通过一个世界坐标点，获取离它最近的两根轴网。

```javascript
// object为鼠标点击事件回调参数中的构件信息
// object.worldPosition为点击位置的世界坐标
// 第二个传参是fileId，只有集成模型需要输入其中单个文件的ID
model3D.getNearestAxisGrids(object.worldPosition, "", function (data) {
  // data为数组，包含最近的两根轴网信息
  // data[0].name、data[1].name分别为两根轴网的名称
  console.log("最近的两根轴网: ", data[0].name, data[1].name);
});
```
