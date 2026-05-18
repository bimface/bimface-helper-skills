# 房间操作

## 使用约束与说明
- 所有房间操作必须在`ViewAdded`事件触发之后进行
- 创建房间时，roomId必须全局唯一
- 创建或修改房间边界时，边界点必须至少包含3个点，且不能形成自交或自相交的多边形
- 房间的生成涉及复杂几何计算，创建操作必须包裹try-catch。任何新增、更新、删除操作后，必须调用`viewer3D.render()`刷新场景
- `getAreas()`、`getComponents()`、`getRoomsByComponentId()` 为异步回调，结果必须在回调函数中处理
- 不需要的房间及时移除，避免内存泄漏

## 获取房间管理器

```javascript
const roomManager = viewer3D.getRoomManager();
```

## 从模型获取房间数据（异步）

```javascript
// 获取模型中的空间区域数据，用于基于模型已有房间信息创建房间
viewer3D.getModel().getAreas(function (roomInfoList) {
    // roomInfoList 包含楼层及对应的房间边界信息
    const boundary_rooms = roomInfoList[1].rooms[0].boundary;
    // areas也可以将它变为房间，其数据类型与rooms是相同的
    const boundary_areas = roomInfoList[1].areas[0].boundary;
});
```

## 创建房间

### 通过边界点创建

```javascript
try {
  const roomManager = viewer3D.getRoomManager();

  const boundaryPoints = [
    { x: -8000.000, y: 20000.000, z: 0.000 },
    { x: 7000.000, y: 20000.000, z: 0.000 },
    { x: 7000.000, y: -12500.000, z: 0.000 },
    { x: -8000.000, y: -12500.000, z: 0.000 }
  ];

  const roomConfig = new Glodon.Bimface.Plugins.Rooms.RoomConfig();
  roomConfig.viewer = viewer3D;
  roomConfig.roomId = 'room1';
  roomConfig.geometry = {
    type: "extrusion",
    // 自定义点位边界时的表述方法
    boundary: { outer: boundaryPoints },
    // 根据getAreas获取边界的表述方法
    // boundary: boundary_areas,
    height: 3000
  };
  roomConfig.roomColor = new Glodon.Web.Graphics.Color(97, 179, 191, 0.2); // (参数格式必须严格为: R, G, B, Alpha 透明度)
  roomConfig.frameColor = new Glodon.Web.Graphics.Color(97, 179, 191, 1); // (参数格式必须严格为: R, G, B, Alpha 透明度)

  const room = new Glodon.Bimface.Plugins.Rooms.Room(roomConfig);
  roomManager.addRoom(room);
  viewer3D.render();
} catch (error) {
  console.error('创建房间失败:', error);
}
```

### 通过轴网创建

```javascript
const roomConfig = new Glodon.Bimface.Plugins.Rooms.RoomConfig();
roomConfig.viewer = viewer3D;
roomConfig.geometry = {
    type: "extrusion",
    grid: {
        ids: ['A', 'E', '1', '3']  // 轴网编号，沿轴网围合形成房间
    },
    offset: [0, 3500]  // [底标高偏移, 顶标高偏移]
};
roomConfig.roomColor = new Glodon.Web.Graphics.Color(255, 0, 0, 0.2);
roomConfig.frameColor = new Glodon.Web.Graphics.Color(255, 0, 0, 1);

const room = new Glodon.Bimface.Plugins.Rooms.Room(roomConfig);
viewer3D.getRoomManager().addRoom(room);
viewer3D.getRoomManager().showAllRooms();
```

## 获取房间对象

```javascript
const roomManager = viewer3D.getRoomManager();

// 获取所有房间对象列表（返回 Room 实例数组）
const allRooms = roomManager.getAllRooms();

// 获取所有房间
const rooms = roomManager.getRooms();

// 根据ID获取指定房间
const room = roomManager.getRoomById("room1");
```

## 获取房间信息

```javascript
const room = roomManager.getRoomById("room1");

// 获取房间底面积
const bottomArea = room.getArea();

// 获取房间边界
const boundary = room.getBoundary();

// 获取房间高度
const height = room.getHeight();

// 获取房间属性
const roomProperties = room.getProperty(function (data) {
  // data 为房间属性对象
  console.log(data);
});

// 获取房间颜色
const roomColor = room.getRoomColor();

// 获取房间框架颜色
const frameColor = room.getRoomFrameColor();

// 获取房间包围盒
const boundingBox = room.getRoomBoundingBox();
```

## 更新房间颜色与高度

```javascript
const roomManager = viewer3D.getRoomManager();
const room = roomManager.getRoomById("room1");
if (room) {
  room.setHeight(3500);
  room.setRoomColor(new Glodon.Web.Graphics.Color(255, 165, 0, 0.2));
  room.setRoomFrameColor(new Glodon.Web.Graphics.Color(255, 165, 0, 1));
  viewer3D.render();
}
```

## 房间显示控制

```javascript
const roomManager = viewer3D.getRoomManager();

// 隐藏指定房间
roomManager.hideRoomsById(["room1", "room2"]);

// 显示指定房间
roomManager.showRoomsById(["room1"]);

// 隐藏所有房间
roomManager.hideAllRooms();

// 显示所有房间
roomManager.showAllRooms();

viewer3D.render();
```

## 空间关系计算（异步）

### 获取房间内构件

```javascript
// mode: 'ORDINARY' | 'LENIENT' | 'STRICT' — 空间关系判断模式
// strictness: 'ORDINARY' | 'LENIENT' | 'STRICT' — 严格程度
roomManager.getRoomById('room1').getComponents('ORDINARY', 'STRICT', function (data) {
    viewer3D.getModel().isolateComponentsById(data, Glodon.Bimface.Viewer.IsolateOption.MakeOthersTranslucent);
    viewer3D.render();
});
```

### 获取构件所在房间

```javascript
const componentId = "735739";
// 查询指定构件所在的房间
roomManager.getRoomsByComponentId(componentId, 'ORDINARY', 'STRICT', function (data) {
    // data 为房间信息数组，data[0].roomId 即构件所在房间ID
    roomManager.showRoomsById([data[0].roomId]);
    viewer3D.render();
});
```

## 判断点是否在房间内

```javascript
const point = { x: -4948, y: 10077, z: 1000 };
// isPointInside(point, includeBoundary)：含边界时第二个参数为true
const isInside = roomManager.getRoomById('bufferArea').isPointInside(point, true);
```

## 深度检测

开启后房间与模型构件之间存在正确遮挡显示关系。配合隐藏遮挡构件以直观展示。

```javascript
const room = roomManager.getRoomById('room1');
room.enableDepthTest(true);

// 配合隐藏被遮挡构件来展示效果
room.getComponents('LENIENT', 'LENIENT', function (data) {
    viewer3D.getModel().hideComponentsById(data);
});
viewer3D.render();

// 关闭深度检测
room.enableDepthTest(false);
viewer3D.render();
```

## 房间编辑器

提供房间编辑UI（调整边界顶点等），通过工具条控制编辑生命周期。

```javascript
// `RoomEditorToolbar` 自身包含保存/取消按钮，无需在外部额外添加
const roomEditorToolbarConfig = new Glodon.Bimface.Plugins.Rooms.RoomEditorToolbarConfig();
roomEditorToolbarConfig.viewer = viewer3D;
roomEditorToolbarConfig.roomId = viewer3D.getRoomManager().getAllRooms()[0].roomId;

const roomEditorToolbar = new Glodon.Bimface.Plugins.Rooms.RoomEditorToolbar(roomEditorToolbarConfig);

roomEditorToolbar.addEventListener(
    Glodon.Bimface.Plugins.Rooms.RoomEditorToolbarEvent.Saved,
    onRoomEditorSaved
);
roomEditorToolbar.addEventListener(
    Glodon.Bimface.Plugins.SpatialRelation.RoomEditorToolbarEvent.Cancelled,
    exitRoomEditor
);

// 显示编辑器工具条
app3D.getToolbar("MainToolbar").hide();
roomEditorToolbar.show();
```

## 移除指定的房间

```JavaScript
const roomManager = viewer3D.getRoomManager();

// 根据房间ID移除
roomManager.clearRoomsById(["room1"]);

viewer3D.render();
```

## 移除全部房间

```JavaScript
const roomManager = viewer3D.getRoomManager();

// 移除全部房间
roomManager.clearAllRooms();

viewer3D.render();
```