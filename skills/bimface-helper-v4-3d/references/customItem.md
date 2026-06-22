# 自定义标签（CustomItem）

## 使用约束与说明
- 自定义标签（CustomItem）是 3D 场景中渲染的 2D HTML 标签，可显示文本、图片等自定义 DOM 内容
- CustomItem 必须先添加到 `DrawableContainer` 容器才能显示，容器创建方式参考 [Drawable容器](drawableContainer.md)
- v4 去除了 `CustomItemConfig` 类，构造函数直接接收对象参数
- 命名空间从 `Plugins.Drawable` 改为 `Plugin.Drawable`（单数）
- 部分属性（content、height、width 等）聚合到 `visualization` 对象中，不再平铺
- 事件监听采用 `addEventListener` / `removeEventListener` 模式，不再使用 `.onClick()` / `.onEndDrag()` 等回调方式
- `position` 对 3D 模型为 `{x, y, z}` 三坐标，对 2D 图纸为 `{x, y}` 二坐标
- `enableDepthTest` 和 `visibleDistance` 仅适用于 Viewer3D

---

## 构造参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| id | `String` | 否 | 标签唯一标识，默认随机生成 |
| position | `{x, y}` 或 `{x, y, z}` | 是 | 标签坐标 |
| visualization | `Object` | 是 | 标签可视化配置 |
| visualization.content | `DOMElement` | 是 | 标签的 HTML 内容（DOM 元素） |
| visualization.height | `Number` | 是 | 标签高度（px） |
| visualization.width | `Number` | 是 | 标签宽度（px） |
| visualization.opacity | `Number` | 否 | 不透明度 [0, 1]，默认 0.75 |
| visualization.tooltip | `String` | 否 | 悬浮提示文本 |
| visualization.tooltipStyle | `Object` | 否 | 提示样式 |
| visualization.angle | `Number` | 否 | 旋转角度 |
| visualization.offset | `{x, y}` | 否 | 沿 X、Y 轴的偏移量（px） |
| draggable | `Boolean` | 否 | 是否允许拖拽，默认 `false` |
| enableDepthTest | `Boolean` | 否 | 是否开启深度检测（Viewer3D），默认 `false` |
| visibleDistance | `Number` | 否 | 可见距离阈值（Viewer3D），不设置则不限制 |

```javascript
// 创建标签内容 DOM
const labelContent = document.createElement("div");
labelContent.innerHTML = `
    <div style="padding: 8px 12px; background: rgba(0,0,0,0.75); color: #fff; 
                border-radius: 4px; font-size: 14px; white-space: nowrap;">
        风机盘管 FCU-2F-12<br/>
        <span style="color: #4CAF80;">● 运行中</span>
    </div>
`;

// 创建 CustomItem（3D 模型场景）
const customItem = new Glodon.Bimface.Plugin.Drawable.CustomItem({
    id: "label-fcu-2f-12",
    position: { x: 5000, y: 8000, z: 3500 },
    visualization: {
        content: labelContent,
        height: 50,
        width: 160,
        opacity: 1.0,
        tooltip: "点击查看详情",
        angle: 0,
        offset: { x: 0, y: -30 }
    },
    draggable: false,
    enableDepthTest: true,
    visibleDistance: 20000
});
```

---

## 事件监听

事件通过 `addEventListener` / `removeEventListener` 注册和移除，事件类型由 `Glodon.Bimface.Plugin.Drawable.DrawableEvent` 定义：

| 事件 | 说明 |
|------|------|
| `DrawableEvent.Click` | 点击事件（含左键/右键） |
| `DrawableEvent.DragEnd` | 拖拽结束事件（需 `draggable: true`） |
| `DrawableEvent.ObstructionChanged` | 遮挡状态变化事件（需 `enableDepthTest: true`，仅 Viewer3D） |

### Click 事件

```javascript
customItem.addEventListener(
    Glodon.Bimface.Plugin.Drawable.DrawableEvent.Click,
    (data) => {
        // data 结构：
        // {
        //     eventType: "Click",          // "Click"（左键）| "RightClick"（右键）
        //     id: "label-fcu-2f-12",       // 标签ID
        //     item: {
        //         id: "label-fcu-2f-12",   // 标签ID
        //         name: "customItem1",      // 标签名称
        //         viewer: viewer3D,         // 所属 Viewer
        //         worldPosition: { x, y, z } // 标签坐标
        //     }
        // }

        if (data.eventType === "Click") {
            console.log("左键点击", data.item.worldPosition);
        } else if (data.eventType === "RightClick") {
            console.log("右键点击", data.id);
        }
    }
);
```

### DragEnd 事件

拖拽结束后返回标签对象的属性：

```javascript
customItem.addEventListener(
    Glodon.Bimface.Plugin.Drawable.DrawableEvent.DragEnd,
    (data) => {
        // data 结构（标签对象）：
        // {
        //     id: "label-fcu-2f-12",
        //     name: "customItem1",
        //     viewer: viewer3D,
        //     worldPosition: { x, y, z }  // 拖拽后的新坐标
        // }
        console.log("拖拽结束，新位置:", data.worldPosition);
    }
);
```

### ObstructionChanged 事件

仅在 `enableDepthTest: true` 时有效，标签被遮挡返回 `true`，否则返回 `false`：

```javascript
customItem.addEventListener(
    Glodon.Bimface.Plugin.Drawable.DrawableEvent.ObstructionChanged,
    (isObstructed) => {
        console.log("标签被遮挡:", isObstructed);  // true 或 false
    }
);
```

### 移除事件监听

```javascript
const clickHandler = (data) => { /* ... */ };
customItem.addEventListener(
    Glodon.Bimface.Plugin.Drawable.DrawableEvent.Click,
    clickHandler
);

// 移除
customItem.removeEventListener(
    Glodon.Bimface.Plugin.Drawable.DrawableEvent.Click,
    clickHandler
);
```

---

## 实例方法

### getVisualization()
获取标签可视化配置。

**返回值**：`{ content, height, width, opacity, tooltip, tooltipStyle, angle, offset }`

```javascript
const vis = customItem.getVisualization();
console.log(vis.width, vis.height, vis.opacity);
```

### updateVisualization(option)
更新标签可视化配置（支持部分更新）。

| 参数 | 类型 | 说明 |
|------|------|------|
| option.content | `DOMElement` | HTML 内容 |
| option.height | `Number` | 标签高度 |
| option.width | `Number` | 标签宽度 |
| option.opacity | `Number` | 不透明度 |
| option.tooltip | `String` | 提示文本 |
| option.tooltipStyle | `Object` | 提示样式 |
| option.angle | `Number` | 旋转角度 |
| option.offset | `{x, y}` | 偏移量 |

```javascript
customItem.updateVisualization({
    opacity: 0.5,
    tooltip: "状态：已停用"
});
```

---

## 完整使用示例

```javascript
// 1. 创建 Drawable 容器（CustomItem 必须添加到容器中才能显示）
const drawableContainer = new Glodon.Bimface.Plugin.Drawable.DrawableContainer({
    viewer: viewer3D
});

// 2. 模型加载完成后创建 CustomItem
viewer3D.addEventListener(
    Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
    () => {
        // 3. 构建标签 DOM 内容
        const content = document.createElement("div");
        content.style.cssText = 
            "padding:6px 10px;background:#1890ff;color:#fff;" +
            "border-radius:4px;font-size:12px;cursor:pointer;";
        content.textContent = "设备：冷水机组 CH-01";

        // 4. 创建 CustomItem
        const item = new Glodon.Bimface.Plugin.Drawable.CustomItem({
            id: "label-ch-01",
            position: { x: 3000, y: 5000, z: 2000 },
            visualization: {
                content: content,
                height: 30,
                width: 180,
                opacity: 0.9,
                tooltip: "点击查看属性",
                angle: 0,
                offset: { x: 0, y: -20 }
            },
            draggable: true,
            enableDepthTest: true,
            visibleDistance: 15000
        });

        // 5. 注册点击事件
        item.addEventListener(
            Glodon.Bimface.Plugin.Drawable.DrawableEvent.Click,
            (data) => {
                if (data.eventType === "Click") {
                    console.log("点击标签:", data.id);
                    // 可在此处调用属性查询等操作
                }
            }
        );

        // 6. 注册拖拽结束事件
        item.addEventListener(
            Glodon.Bimface.Plugin.Drawable.DrawableEvent.DragEnd,
            (data) => {
                console.log("标签被拖拽至:", data.worldPosition);
            }
        );

        // 7. 注册遮挡状态变化事件
        item.addEventListener(
            Glodon.Bimface.Plugin.Drawable.DrawableEvent.ObstructionChanged,
            (isObstructed) => {
                // 被遮挡时降低不透明度
                item.updateVisualization({ opacity: isObstructed ? 0.3 : 0.9 });
            }
        );

        // 8. 将标签添加到容器
        drawableContainer.addItem(item);
    }
);
```
