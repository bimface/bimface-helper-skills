# 房间（Room）

## 使用约束与说明
- 房间类名为 `Glodon.Bimface.Plugin.Room.RoomItem`（注意是 `Room` 单数，类名是 `RoomItem` 不是 `Room`）
- 所有房间操作必须在 `ViewAdded` 事件触发后进行
- 房间 ID 必须全局唯一
- 创建或修改房间边界时，边界点至少包含 3 个点，且不能形成自交多边形
- v4 去除了 `RoomConfig` 类，构造函数直接接收对象参数
- 命名空间是 `Plugin`（单数），不是 `Plugins`
- 颜色必须使用 `new Glodon.Bimface.Common.Graphics.Color(R, G, B, Alpha)` 构造
- 房间通过 `modelId` 绑定模型（已废弃 `bindRoomByModelId()`）
- `getProperty()` 和 `getComponents()` 返回 Promise，必须使用 `.then()` 或 `async/await` 处理
- `getGeometry()` 统一返回边界/网格和高度
- `getColor()` 统一返回房间色和边框色
- 房间创建、更新、删除后必须调用 `viewer3D.render()` 刷新场景
- 房间数据可通过 `model.getAreas()` 获取 Promise，返回 `[{rooms: [{id, boundary}]}]`

---

## 创建房间

### 构造参数

`new Glodon.Bimface.Plugin.Room.RoomItem({viewer, id?, modelId?, geometry, color, selectable?})`

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| viewer | `Object` | 是 | Viewer3D 实例 |
| id | `String` | 否 | 房间唯一标识，默认自动生成 |
| modelId | `String` | 否 | 绑定的模型 ID（替代旧版 `bindRoomByModelId`） |
| geometry | `Object` | 是 | 几何配置，支持 boundary 或 grid 两种模式 |
| geometry.boundary | `Object` | 否 | 边界模式配置（与 grid 二选一） |
| geometry.boundary.outer | `Array<{x,y,z}>` | 是 | 外边界点数组，至少 3 个点 |
| geometry.boundary.inner | `Array<{x,y,z}>` | 否 | 内边界（孔洞）点数组，默认空 |
| geometry.boundary.coordinateSpace | `"Model"` \| `"Scene"` | 否 | 坐标空间，默认 `"Model"` |
| geometry.grid | `Object` | 否 | 网格模式配置（与 boundary 二选一） |
| geometry.grid.ids | `String[]` | 是 | 网格 ID 数组（轴网 grid 模式） |
| geometry.height | `Number` | 是 | 房间高度 |
| geometry.unit | `"mm"` \| `"cm"` \| `"m"` 等 | 否 | 高度单位 |
| color | `Object` | 是 | 颜色配置 |
| color.roomColor | `Color` | 是 | 房间填充色，`new Glodon.Bimface.Common.Graphics.Color(R,G,B,A)` |
| color.frameColor | `Color` | 是 | 房间边框色 |
| selectable | `Boolean` | 否 | 是否可选中，默认 `true` |

### 示例一：基于边界（boundary）创建房间

```javascript
const room = new Glodon.Bimface.Plugin.Room.RoomItem({
    viewer: viewer3D,
    id: "room-101",
    modelId: "model_123456",
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
```

### 示例二：基于轴网 ID（grid）创建房间

```javascript
const room = new Glodon.Bimface.Plugin.Room.RoomItem({
    viewer: viewer3D,
    id: "room-grid-01",
    modelId: "model_123456",
    geometry: {
        grid: {
            ids: ["grid-1", "grid-2", "grid-3", "grid-4"]
        },
        height: 3500,
        unit: "mm"
    },
    color: {
        roomColor: new Glodon.Bimface.Common.Graphics.Color(255, 200, 100, 0.2),
        frameColor: new Glodon.Bimface.Common.Graphics.Color(255, 200, 100, 0.9)
    },
    selectable: true
});
```

---

## 从模型数据创建房间（model.getAreas）

`model.getAreas()` 返回 Promise，解析为房间信息数组，可用于批量创建房间：

```javascript
model.getAreas().then((areas) => {
    areas.forEach((area) => {
        // area 结构: { rooms: [{ id, boundary }] }
        if (area.rooms) {
            area.rooms.forEach((roomData) => {
                const room = new Glodon.Bimface.Plugin.Room.RoomItem({
                    viewer: viewer3D,
                    id: roomData.id,
                    modelId: model.getId(),
                    geometry: {
                        boundary: {
                            outer: roomData.boundary,
                            inner: [],
                            coordinateSpace: "Model"
                        },
                        height: 3000,
                        unit: "mm"
                    },
                    color: {
                        roomColor: new Glodon.Bimface.Common.Graphics.Color(0, 128, 255, 0.15),
                        frameColor: new Glodon.Bimface.Common.Graphics.Color(0, 128, 255, 0.8)
                    }
                });
                // 添加到 RoomManager
                roomManager.addRoom(room);
            });
        }
    });
    viewer3D.render();
});
```

---

## 实例方法

### getGeometry()
获取房间的几何信息。

**返回值**：`{ boundary?, grid?, height }`

```javascript
const geometry = room.getGeometry();
// 返回 { boundary: { outer: [...], inner: [...], coordinateSpace: "Model" }, height: 3000 }
// 或基于 grid: { grid: { ids: [...] }, height: 3500 }

if (geometry.boundary) {
    console.log("边界点:", geometry.boundary.outer);
}
if (geometry.grid) {
    console.log("网格 ID:", geometry.grid.ids);
}
console.log("高度:", geometry.height);
```

### setGeometry(option)
设置房间的几何信息（支持部分更新，可切换 boundary/grid 模式）。

| 参数 | 类型 | 说明 |
|------|------|------|
| option.boundary | `Object` | 边界配置（含 outer、inner、coordinateSpace） |
| option.grid | `Object` | 网格配置（含 ids） |
| option.height | `Number` | 房间高度 |
| option.unit | `String` | 高度单位 |

```javascript
// 更新边界
room.setGeometry({
    boundary: {
        outer: [
            { x: 0, y: 0, z: 0 },
            { x: 8000, y: 0, z: 0 },
            { x: 8000, y: 10000, z: 0 },
            { x: 0, y: 10000, z: 0 }
        ],
        inner: [],
        coordinateSpace: "Scene"
    }
});

// 仅更新高度
room.setGeometry({
    height: 3500,
    unit: "mm"
});

// 切换到 grid 模式
room.setGeometry({
    grid: { ids: ["grid-A", "grid-B", "grid-C", "grid-D"] },
    height: 4000,
    unit: "mm"
});

viewer3D.render();
```

### getColor()
获取房间颜色配置。

**返回值**：`{ roomColor: Color, frameColor: Color }`

```javascript
const color = room.getColor();
const roomColor = color.roomColor;     // Glodon.Bimface.Common.Graphics.Color
const frameColor = color.frameColor;   // Glodon.Bimface.Common.Graphics.Color
```

### setColor(option)
设置房间颜色（支持部分更新）。

| 参数 | 类型 | 说明 |
|------|------|------|
| option.roomColor | `Color` | 房间填充色 |
| option.frameColor | `Color` | 房间边框色 |

```javascript
// 同时更新填充色和边框色
room.setColor({
    roomColor: new Glodon.Bimface.Common.Graphics.Color(255, 0, 0, 0.3),
    frameColor: new Glodon.Bimface.Common.Graphics.Color(255, 0, 0, 1.0)
});

// 只更新边框色
room.setColor({
    frameColor: new Glodon.Bimface.Common.Graphics.Color(0, 128, 255, 0.8)
});

viewer3D.render();
```

### getProperty()
获取房间属性，返回 Promise。

**返回值**：`Promise<Object>` — 房间属性对象

```javascript
room.getProperty().then((property) => {
    console.log("房间属性:", property);
});

// 或使用 async/await
const property = await room.getProperty();
console.log(property);
```

### getComponents(option)
获取房间内的构件，返回 Promise。

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| option.toleranceXY | `String` | 是 | XY 平面容差：`"LENIENT"` / `"ORDINARY"` / `"STRICT"` |
| option.toleranceZ | `String` | 是 | Z 方向容差：`"LENIENT"` / `"ORDINARY"` / `"STRICT"` |

**返回值**：`Promise<String[]>` — 构件 ID 数组

```javascript
room.getComponents({
    toleranceXY: "ORDINARY",
    toleranceZ: "STRICT"
}).then((componentIds) => {
    console.log("房间内构件:", componentIds);
});
```

### enableDepthTest(enable)
切换房间与构件的遮挡关系（深度测试）。

| 参数 | 类型 | 说明 |
|------|------|------|
| enable | `Boolean` | `true` 开启深度测试（构件可遮挡房间），`false` 关闭（房间始终可见） |

```javascript
// 开启深度测试：房间被前方构件遮挡
room.enableDepthTest(true);

// 关闭深度测试：房间在所有构件之上可见
room.enableDepthTest(false);

viewer3D.render();
```

---

## 完整使用示例

### 边界模式完整流程

```javascript
viewer3D.addEventListener(
    Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
    async () => {
        // 1. 获取模型数据中的房间信息并创建
        model.getAreas().then((areas) => {
            areas.forEach((area) => {
                if (area.rooms) {
                    area.rooms.forEach((roomData) => {
                        const room = new Glodon.Bimface.Plugin.Room.RoomItem({
                            viewer: viewer3D,
                            id: roomData.id,
                            geometry: {
                                boundary: {
                                    outer: roomData.boundary,
                                    inner: [],
                                    coordinateSpace: "Model"
                                },
                                height: 3000,
                                unit: "mm"
                            },
                            color: {
                                roomColor: new Glodon.Bimface.Common.Graphics.Color(0, 128, 255, 0.15),
                                frameColor: new Glodon.Bimface.Common.Graphics.Color(0, 128, 255, 0.8)
                            },
                            selectable: true
                        });
                        roomManager.addRoom(room);
                    });
                }
            });
            viewer3D.render();
        });

        // 2. 手动创建边界房间
        const room = new Glodon.Bimface.Plugin.Room.RoomItem({
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
                height: 4500,
                unit: "mm"
            },
            color: {
                roomColor: new Glodon.Bimface.Common.Graphics.Color(100, 200, 255, 0.15),
                frameColor: new Glodon.Bimface.Common.Graphics.Color(100, 200, 255, 0.8)
            },
            selectable: true
        });
        roomManager.addRoom(room);

        // 3. 获取房间内构件
        try {
            const componentIds = await room.getComponents({
                toleranceXY: "LENIENT",
                toleranceZ: "ORDINARY"
            });
            console.log(`房间内共 ${componentIds.length} 个构件`);
        } catch (err) {
            console.error("获取房间内构件失败:", err);
        }

        // 4. 更新房间颜色和高度
        room.setColor({
            frameColor: new Glodon.Bimface.Common.Graphics.Color(255, 100, 100, 0.9)
        });
        room.setGeometry({ height: 5000, unit: "mm" });

        viewer3D.render();
    }
);
```

### 轴网模式完整示例

```javascript
viewer3D.addEventListener(
    Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
    async () => {
        // 基于轴网 ID 创建房间
        const gridRoom = new Glodon.Bimface.Plugin.Room.RoomItem({
            viewer: viewer3D,
            id: "room-grid-lobby",
            geometry: {
                grid: {
                    ids: ["grid-a1", "grid-a4", "grid-d4", "grid-d1"]
                },
                height: 4000,
                unit: "mm"
            },
            color: {
                roomColor: new Glodon.Bimface.Common.Graphics.Color("#EE799F", 0.3),
                frameColor: new Glodon.Bimface.Common.Graphics.Color("#EE799F", 1.0)
            },
            selectable: true
        });
        roomManager.addRoom(gridRoom);

        // 开启深度测试
        gridRoom.enableDepthTest(true);

        viewer3D.render();

        // 获取房间属性
        const prop = await gridRoom.getProperty();
        console.log("房间属性:", prop);
    }
);
```
