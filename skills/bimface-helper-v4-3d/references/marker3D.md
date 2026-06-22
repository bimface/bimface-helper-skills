# 三维标签（Marker3D）

## 使用约束与说明
- 所有 Marker3D 操作必须在 `ViewAdded` 事件触发后进行
- 必须先创建 `Marker3DContainer` 容器，再创建 `Marker3DItem` 标签对象并添加到容器中
- v4 中 `Marker3D` 类已更名为 `Marker3DItem`，命名空间从 `Plugins.Marker3D` 改为 `Plugin.Marker3D`（单数）
- v4 去除了 `Marker3DContainerConfig` 和 `Marker3DConfig` 类，构造函数直接接收对象参数
- 事件监听采用 `addEventListener` / `removeEventListener` 模式，不再使用 `.onClick()` / `.onHover()` 等回调方式
- `addItems()` 接收数组（非单个对象），代替旧版 `addItem()`
- 点击事件统一由 `Marker3DEvent.Click` 处理，通过回调参数的 `eventType` 字段区分左右键（`"Click"` / `"RightClick"`）

---

## 创建三维标签容器（Marker3DContainer）

```javascript
// 创建 Marker3DContainer，必须传入 viewer
const markerContainer = new Glodon.Bimface.Plugin.Marker3D.Marker3DContainer({
    viewer: viewer3D
});
```

### 容器方法

#### addItems(items)
批量添加三维标签到容器中。

| 参数 | 类型 | 说明 |
|------|------|------|
| items | `Marker3DItem[]` | 三维标签对象数组 |

```javascript
markerContainer.addItems([markerItem1, markerItem2]);
```

#### clear()
清空容器中所有三维标签。

```javascript
markerContainer.clear();
```

#### getAllItems()
获取容器中所有三维标签。

**返回值**：`Marker3DItem[]`

```javascript
const allMarkers = markerContainer.getAllItems();
```

#### getItemById(id)
根据ID获取三维标签对象。

| 参数 | 类型 | 说明 |
|------|------|------|
| id | `String` | 三维标签ID |

**返回值**：`Marker3DItem`

```javascript
const marker = markerContainer.getItemById("marker1");
```

#### hideItems(condition)
根据条件隐藏三维标签。

| 参数 | 类型 | 说明 |
|------|------|------|
| condition.all | `Boolean` | 是否隐藏全部，默认 `false` |
| condition.ids | `String[]` | 要隐藏的标签ID数组，默认空数组 |

```javascript
// 隐藏全部
markerContainer.hideItems({ all: true });

// 隐藏指定ID
markerContainer.hideItems({ ids: ["marker1", "marker2"] });
```

#### showItems(condition)
根据条件显示三维标签。

| 参数 | 类型 | 说明 |
|------|------|------|
| condition.all | `Boolean` | 是否显示全部，默认 `false` |
| condition.ids | `String[]` | 要显示的标签ID数组，默认空数组 |

```javascript
// 显示全部
markerContainer.showItems({ all: true });

// 显示指定ID
markerContainer.showItems({ ids: ["marker1"] });
```

#### removeItemById(id)
根据ID移除三维标签。

| 参数 | 类型 | 说明 |
|------|------|------|
| id | `String` | 三维标签ID |

```javascript
markerContainer.removeItemById("marker1");
```

#### update()
刷新容器内所有三维标签的渲染。

```javascript
markerContainer.update();
```

---

## 创建三维标签对象（Marker3DItem）

### 构造参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| id | `String` | 否 | 标签唯一标识，默认随机生成 |
| worldPosition | `{x, y, z}` | 是 | 标签三维空间坐标 |
| visualization | `Object` | 是 | 标签可视化配置 |
| visualization.url | `String` | 是 | 标签图标URL |
| visualization.size | `Number` | 否 | 图标大小（像素） |
| visualization.hoverAnimation | `Boolean` | 否 | 是否开启hover动画 |
| visualization.tooltip | `String` | 否 | 悬浮提示文本 |
| visualization.tooltipStyle | `Object` | 否 | 提示样式 |

```javascript
// 创建单个 Marker3DItem
const markerItem = new Glodon.Bimface.Plugin.Marker3D.Marker3DItem({
    id: "marker1",
    worldPosition: { x: 100, y: 200, z: 300 },
    visualization: {
        url: "./markerIcon.png",
        size: 24,
        hoverAnimation: true,
        tooltip: "设备编号：AHU-001",
        tooltipStyle: { fontSize: "12px", color: "#333" }
    }
});
```

### 实例方法

#### addEventListener(event, callback)
注册事件监听器。

| 参数 | 类型 | 说明 |
|------|------|------|
| event | `Marker3DEvent` | 事件类型 |
| callback | `Function` | 事件回调函数 |

**事件类型**：
- `Glodon.Bimface.Plugin.Marker3D.Marker3DEvent.Click` — 点击事件（含左右键）
- `Glodon.Bimface.Plugin.Marker3D.Marker3DEvent.Hover` — 悬浮事件

```javascript
// 注册点击事件（左键和右键统一由 Click 处理）
markerItem.addEventListener(
    Glodon.Bimface.Plugin.Marker3D.Marker3DEvent.Click,
    (data) => {
        if (data.eventType === "Click") {
            console.log("左键点击", data);
        } else if (data.eventType === "RightClick") {
            console.log("右键点击", data);
        }
    }
);

// 注册悬浮事件
markerItem.addEventListener(
    Glodon.Bimface.Plugin.Marker3D.Marker3DEvent.Hover,
    (data) => {
        console.log("Hover", data);
    }
);
```

**Click 事件回调参数结构**：

```javascript
{
    container: container,          // Marker3DContainer 容器实例
    eventType: "Click",           // "Click"（左键）| "RightClick"（右键）
    id: "marker1",                // 标签ID
    isHideByClustering: false,    // 是否因聚合而隐藏
    position: { x: 100, y: 200, z: 300 }  // 标签坐标
}
```

#### removeEventListener(event, callback)
移除事件监听器。

```javascript
const handler = (data) => { /* ... */ };
markerItem.removeEventListener(
    Glodon.Bimface.Plugin.Marker3D.Marker3DEvent.Click,
    handler
);
```

#### getId()
获取标签ID。

**返回值**：`String`

```javascript
const id = markerItem.getId();
```

#### getWorldPosition()
获取标签三维坐标。

**返回值**：`{ x: Number, y: Number, z: Number }`

```javascript
const pos = markerItem.getWorldPosition();  // { x: 100, y: 200, z: 300 }
```

#### getVisualization()
获取标签可视化配置。

**返回值**：`{ url, size, hoverAnimation, tooltip, tooltipStyle }`

```javascript
const vis = markerItem.getVisualization();
console.log(vis.url, vis.size, vis.tooltip);
```

#### updateVisualization(option)
更新标签可视化配置（支持部分更新）。

| 参数 | 类型 | 说明 |
|------|------|------|
| option.url | `String` | 图标URL |
| option.size | `Number` | 图标大小 |
| option.hoverAnimation | `Boolean` | 是否开启hover动画 |
| option.tooltip | `String` | 提示文本 |
| option.tooltipStyle | `Object` | 提示样式 |

```javascript
markerItem.updateVisualization({
    size: 32,
    tooltip: "状态：运行中"
});
```

#### setWorldPosition(worldPosition)
设置标签的三维空间坐标。

| 参数 | 类型 | 说明 |
|------|------|------|
| worldPosition | `{ x, y, z }` | 新坐标 |

```javascript
markerItem.setWorldPosition({ x: 150, y: 250, z: 350 });
```

#### update()
刷新单个标签的渲染。

```javascript
markerItem.update();
```

---

## 完整使用示例

```javascript
// 1. 模型加载完成后，创建标签容器
viewer3D.addEventListener(
    Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
    () => {
        // 2. 创建 Marker3DContainer
        const markerContainer = new Glodon.Bimface.Plugin.Marker3D.Marker3DContainer({
            viewer: viewer3D
        });

        // 3. 创建多个 Marker3DItem
        const marker1 = new Glodon.Bimface.Plugin.Marker3D.Marker3DItem({
            id: "marker-air-handler",
            worldPosition: { x: 1000, y: 2000, z: 3000 },
            visualization: {
                url: "./icons/hvac.png",
                size: 20,
                hoverAnimation: true,
                tooltip: "空调机组 AHU-1F-01"
            }
        });

        const marker2 = new Glodon.Bimface.Plugin.Marker3D.Marker3DItem({
            id: "marker-pump",
            worldPosition: { x: 2500, y: 1500, z: 3000 },
            visualization: {
                url: "./icons/pump.png",
                size: 20,
                hoverAnimation: false,
                tooltip: "水泵 P-1F-03"
            }
        });

        // 4. 为标签注册点击事件（区分左键/右键）
        const handleClick = (data) => {
            if (data.eventType === "Click") {
                console.log("左键点击了标签:", data.id, "位置:", data.position);
                // 可以跳转到构件或展示详情面板
            } else if (data.eventType === "RightClick") {
                console.log("右键点击了标签:", data.id);
                // 可以弹出右键菜单
            }
        };

        marker1.addEventListener(
            Glodon.Bimface.Plugin.Marker3D.Marker3DEvent.Click,
            handleClick
        );
        marker2.addEventListener(
            Glodon.Bimface.Plugin.Marker3D.Marker3DEvent.Click,
            handleClick
        );

        // 5. 批量添加标签到容器
        markerContainer.addItems([marker1, marker2]);

        // 6. （可选）后续操作示例
        // 隐藏指定标签
        // markerContainer.hideItems({ ids: ["marker-air-handler"] });
        // 显示所有
        // markerContainer.showItems({ all: true });
        // 更新单个标签位置
        // marker1.setWorldPosition({ x: 1200, y: 2100, z: 3000 });
        // markerContainer.update();
    }
);
```
