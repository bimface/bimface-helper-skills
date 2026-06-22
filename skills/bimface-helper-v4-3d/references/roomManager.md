# 房间管理器（RoomManager）

## 使用约束与说明
- RoomManager 用于批量管理场景中所有房间的显示/隐藏、清除、可选中状态及空间关系查询
- **v4 中 RoomManager 通过 `viewer3D.getRoomManager()` 获取，不是 `new RoomManager()` 构造**
- 房间对象类名为 `Glodon.Bimface.Plugin.Room.RoomItem`（注意是 `Room` 单数）
- 所有批量操作方法统一为对象参数模式：`{all: true}` 或 `{ids: [...]}`
- `getRoomsByComponentId()` 返回 Promise，必须使用 `.then()` 或 `async/await` 处理
- 参数中字符串类型的 `toleranceXY` / `toleranceZ` 取值：`"LENIENT"` / `"ORDINARY"` / `"STRICT"`
- `isRoomSelectable()` 替代旧版 `getRoomSelectableById()`
- `setRoomsSelectable()` 替代旧版 `setRoomSelectableById()`

---

## 获取 RoomManager

```javascript
// 通过 viewer3D 获取 RoomManager（不是 new 构造！）
const roomManager = viewer3D.getRoomManager();
```

---

## 实例方法

### addRoom(roomItem)
添加一个房间到场景中。

| 参数 | 类型 | 说明 |
|------|------|------|
| roomItem | `Glodon.Bimface.Plugin.Room.RoomItem` | 房间实例 |

```javascript
// 先创建 RoomItem，再添加到 RoomManager
const room = new Glodon.Bimface.Plugin.Room.RoomItem({
    viewer: viewer3D,
    id: "room-101",
    geometry: {
        boundary: {
            outer: [
                { x: 0, y: 0, z: 0 },
                { x: 6000, y: 0, z: 0 },
                { x: 6000, y: 8000, z: 0 },
                { x: 0, y: 8000, z: 0 }
            ],
            inner: [],
            coordinateSpace: "Model"
        },
        height: 3000,
        unit: "mm"
    },
    color: {
        roomColor: new Glodon.Bimface.Common.Graphics.Color(97, 179, 191, 0.2),
        frameColor: new Glodon.Bimface.Common.Graphics.Color(97, 179, 191, 1.0)
    },
    selectable: true
});

roomManager.addRoom(room);
viewer3D.render();
```

### getRoomById(id)
根据 ID 获取房间实例。

| 参数 | 类型 | 说明 |
|------|------|------|
| id | `String` | 房间 ID |

**返回值**：`Glodon.Bimface.Plugin.Room.RoomItem | undefined`

```javascript
const room = roomManager.getRoomById("room-101");
if (room) {
    // 修改房间颜色
    room.setColor({
        frameColor: new Glodon.Bimface.Common.Graphics.Color(255, 0, 0, 1.0)
    });
    viewer3D.render();
}
```

---

### clearRooms(condition)
清除（移除）房间。

| 参数 | 类型 | 说明 |
|------|------|------|
| condition.all | `Boolean` | 是否清除全部，默认 `false` |
| condition.ids | `String[]` | 要清除的房间 ID 数组，默认空数组 |

```javascript
// 清除全部房间
roomManager.clearRooms({ all: true });

// 清除指定房间
roomManager.clearRooms({ ids: ["room-lobby", "room-101"] });

viewer3D.render();
```

### hideRooms(condition)
隐藏房间。

| 参数 | 类型 | 说明 |
|------|------|------|
| condition.all | `Boolean` | 是否隐藏全部，默认 `false` |
| condition.ids | `String[]` | 要隐藏的房间 ID 数组，默认空数组 |

```javascript
// 隐藏全部房间
roomManager.hideRooms({ all: true });

// 隐藏指定房间
roomManager.hideRooms({ ids: ["room-lobby"] });

viewer3D.render();
```

### showRooms(condition)
显示房间。

| 参数 | 类型 | 说明 |
|------|------|------|
| condition.all | `Boolean` | 是否显示全部，默认 `false` |
| condition.ids | `String[]` | 要显示的房间 ID 数组，默认空数组 |

```javascript
// 显示全部房间
roomManager.showRooms({ all: true });

// 显示指定房间
roomManager.showRooms({ ids: ["room-lobby", "room-101"] });

viewer3D.render();
```

### getRoomsByComponentId(option)
根据构件 ID 查询其所在的房间，返回 Promise。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| option.objectId | `String` | 是 | 构件 ID |
| option.toleranceXY | `String` | 是 | XY 平面容差：`"LENIENT"` / `"ORDINARY"` / `"STRICT"` |
| option.toleranceZ | `String` | 是 | Z 方向容差：`"LENIENT"` / `"ORDINARY"` / `"STRICT"` |

**返回值**：`Promise<Array>` — 房间信息数组

```javascript
roomManager.getRoomsByComponentId({
    objectId: "735739",
    toleranceXY: "ORDINARY",
    toleranceZ: "STRICT"
}).then((rooms) => {
    if (rooms && rooms.length > 0) {
        console.log("构件所在房间:", rooms[0].roomId);
        // 显示该房间
        roomManager.showRooms({ ids: [rooms[0].roomId] });
        viewer3D.render();
    }
});

// 或使用 async/await
const rooms = await roomManager.getRoomsByComponentId({
    objectId: "735739",
    toleranceXY: "ORDINARY",
    toleranceZ: "STRICT"
});
```

### isRoomSelectable(roomId)
查询房间是否可被选中。

| 参数 | 类型 | 说明 |
|------|------|------|
| roomId | `String` | 房间 ID |

**返回值**：`Boolean`

```javascript
const selectable = roomManager.isRoomSelectable("room-lobby");
console.log("房间是否可选中:", selectable);
```

### setRoomsSelectable(condition, state)
设置房间的可选中状态。

| 参数 | 类型 | 说明 |
|------|------|------|
| condition.ids | `String[]` | 要操作的房间 ID 数组 |
| state | `Boolean` | 是否可选中 |

```javascript
// 设置指定房间不可选中
roomManager.setRoomsSelectable({ ids: ["room-lobby"] }, false);

// 设置多个房间可选中
roomManager.setRoomsSelectable({ ids: ["room-101", "room-102"] }, true);
```

---

## 完整使用示例

### 边界模式完整流程

```javascript
// 模型加载完成后操作
viewer3D.addEventListener(
    Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
    async () => {
        // 1. 获取 RoomManager（通过 viewer3D，不是 new 构造！）
        const roomManager = viewer3D.getRoomManager();

        // 2. 创建多个房间（类名: Glodon.Bimface.Plugin.Room.RoomItem）
        const room1 = new Glodon.Bimface.Plugin.Room.RoomItem({
            viewer: viewer3D,
            id: "room-lobby",
            geometry: {
                boundary: {
                    outer: [
                        { x: -5000, y: 0, z: 0 },
                        { x: 5000, y: 0, z: 0 },
                        { x: 5000, y: 8000, z: 0 },
                        { x: -5000, y: 8000, z: 0 }
                    ],
                    inner: [],
                    coordinateSpace: "Model"
                },
                height: 4000,
                unit: "mm"
            },
            color: {
                roomColor: new Glodon.Bimface.Common.Graphics.Color(100, 200, 255, 0.15),
                frameColor: new Glodon.Bimface.Common.Graphics.Color(100, 200, 255, 0.8)
            },
            selectable: true
        });

        const room2 = new Glodon.Bimface.Plugin.Room.RoomItem({
            viewer: viewer3D,
            id: "room-office",
            geometry: {
                boundary: {
                    outer: [
                        { x: 6000, y: 0, z: 0 },
                        { x: 12000, y: 0, z: 0 },
                        { x: 12000, y: 8000, z: 0 },
                        { x: 6000, y: 8000, z: 0 }
                    ],
                    inner: [],
                    coordinateSpace: "Model"
                },
                height: 3000,
                unit: "mm"
            },
            color: {
                roomColor: new Glodon.Bimface.Common.Graphics.Color(255, 200, 100, 0.15),
                frameColor: new Glodon.Bimface.Common.Graphics.Color(255, 200, 100, 0.8)
            },
            selectable: true
        });

        // 3. 添加房间到管理器
        roomManager.addRoom(room1);
        roomManager.addRoom(room2);

        // 4. 显示所有房间
        roomManager.showRooms({ all: true });

        // 5. 根据 ID 获取房间
        const lobby = roomManager.getRoomById("room-lobby");
        if (lobby) {
            lobby.setColor({
                frameColor: new Glodon.Bimface.Common.Graphics.Color(255, 0, 0, 1.0)
            });
        }

        // 6. 根据构件 ID 查询所在房间
        try {
            const rooms = await roomManager.getRoomsByComponentId({
                objectId: "735739",
                toleranceXY: "LENIENT",
                toleranceZ: "ORDINARY"
            });

            if (rooms && rooms.length > 0) {
                console.log("构件 735739 位于房间:", rooms[0].roomId);
            }
        } catch (err) {
            console.error("查询构件所在房间失败:", err);
        }

        // 7. 设置房间不可选中
        roomManager.setRoomsSelectable({ ids: ["room-office"] }, false);
        console.log("room-office 是否可选中:", roomManager.isRoomSelectable("room-office"));  // false

        // 8. 隐藏指定房间
        roomManager.hideRooms({ ids: ["room-office"] });

        // 9. 清除全部房间（取消注释即生效）
        // roomManager.clearRooms({ all: true });

        viewer3D.render();
    }
);
```

### 轴网（Grid）模式完整示例

```javascript
viewer3D.addEventListener(
    Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
    async () => {
        const roomManager = viewer3D.getRoomManager();

        // 基于轴网 ID 创建房间
        const gridRoom = new Glodon.Bimface.Plugin.Room.RoomItem({
            viewer: viewer3D,
            id: "room-grid-01",
            geometry: {
                grid: {
                    ids: ["grid-a1", "grid-a4", "grid-d4", "grid-d1"]
                },
                height: 3500,
                unit: "mm"
            },
            color: {
                roomColor: new Glodon.Bimface.Common.Graphics.Color("#EE799F", 0.3),
                frameColor: new Glodon.Bimface.Common.Graphics.Color("#EE799F", 1.0)
            },
            selectable: true
        });

        roomManager.addRoom(gridRoom);
        viewer3D.render();

        // 通过 ID 获取并操作
        const room = roomManager.getRoomById("room-grid-01");
        if (room) {
            room.enableDepthTest(true);
            viewer3D.render();

            const prop = await room.getProperty();
            console.log("房间属性:", prop);
        }
    }
);
```
