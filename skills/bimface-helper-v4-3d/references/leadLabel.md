# 引线标签（LeadLabel）

> ⚠️ **注意**：本文档基于 v3 API 文档 `bimface-helper-v3-3d` 和 v4 通用迁移模式（命名空间 `Plugins→Plugin`、Config 类去除、Color 类路径变更等）编写，具体 API 请以 `参考/接口文档/LeadLabel-v4.pdf` 为准。

## 使用约束与说明

- **命名空间变更**：v4 中命名空间从 `Plugins.Drawable`（复数）变更为 `Plugin.Drawable`（单数）
- **Config 类移除**：不再使用 `LeadLabelConfig` 类，构造函数直接接收配置对象
- 必须在 `ViewAdded` 事件触发后才能添加引线标签
- 引线标签属于 `DrawableContainer` 管理的标签之一，需确保标签容器已初始化（参照 [自定义绘制容器](drawableContainer.md)）
- 标签 ID 必须全局唯一，避免与其他标签冲突
- v4 中使用 `addItems([label])`（数组形式）替代旧版 `addItem(label)`

---

## 创建引线标签

```javascript
let drawableContainer;

// 1. 初始化 DrawableContainer（需在 ViewAdded 事件后执行）
viewer3D.addEventListener(
  Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
  () => {
    drawableContainer = new Glodon.Bimface.Plugin.Drawable.DrawableContainer({
      viewer: viewer3D
    });

    // 2. 创建引线标签对象（无需 Config 类，直接传入配置对象）
    const leadLabel = new Glodon.Bimface.Plugin.Drawable.LeadLabel({
      text: "引线标签",                            // 标签显示文本
      worldPosition: { x: 100, y: 200, z: 300 },   // 三维世界坐标
      viewer: viewer3D,                             // Viewer3D 实例
      id: "leadLabel1"                              // 唯一标识ID
    });

    // 3. 将标签添加到容器（v4 使用 addItems 数组形式）
    drawableContainer.addItems([leadLabel]);

    // 4. 渲染场景
    viewer3D.render();
  }
);
```

## 移除引线标签

```javascript
// 根据 ID 移除指定引线标签
drawableContainer.removeItemById("leadLabel1");

// 清空容器中所有标签
drawableContainer.clear();

viewer3D.render();
```

## 更新引线标签位置

```javascript
// 获取标签对象
const label = drawableContainer.getItemById("leadLabel1");

// 更新世界坐标
label.setWorldPosition({ x: 150, y: 250, z: 350 });

viewer3D.render();
```

## 更新引线标签文本

```javascript
const label = drawableContainer.getItemById("leadLabel1");
label.setText("新的标签文本");
viewer3D.render();
```

---

## v3 → v4 对照

| 项目 | v3（旧版） | v4（新版） |
|------|-----------|-----------|
| 命名空间 | `Plugins.Drawable` | `Plugin.Drawable` |
| Config 类 | `new LeadLabelConfig()` 设置属性 | 不再需要，直接传对象 `{text, worldPosition, viewer, id}` |
| 添加标签 | `drawableContainer.addItem(label)` | `drawableContainer.addItems([label])` |
| Color 类路径 | `Glodon.Web.Graphics.Color` | `Glodon.Bimface.Common.Graphics.Color` |

---

## 完整示例

```javascript
// 假设已初始化 viewer3D
let drawableContainer;

viewer3D.addEventListener(
  Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
  () => {
    // 初始化容器
    drawableContainer = new Glodon.Bimface.Plugin.Drawable.DrawableContainer({
      viewer: viewer3D
    });

    // 批量创建引线标签
    const label1 = new Glodon.Bimface.Plugin.Drawable.LeadLabel({
      text: "设备A",
      worldPosition: { x: 1000, y: 2000, z: 3000 },
      viewer: viewer3D,
      id: "label_device_a"
    });

    const label2 = new Glodon.Bimface.Plugin.Drawable.LeadLabel({
      text: "设备B",
      worldPosition: { x: 2500, y: 2000, z: 3000 },
      viewer: viewer3D,
      id: "label_device_b"
    });

    // 批量添加
    drawableContainer.addItems([label1, label2]);
    viewer3D.render();
  }
);
```
