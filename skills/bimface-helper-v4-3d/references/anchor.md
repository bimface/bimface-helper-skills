# 立体锚点效果（Anchor）

> ⚠️ **注意**：本文档基于 v3 API 文档 `bimface-helper-v3-3d` 和 v4 通用迁移模式（命名空间 `Plugins→Plugin`、Config 类去除等）编写，具体 API 请以 `参考/接口文档/Anchor-v4.pdf` 为准。

## 使用约束与说明

- **命名空间变更**：v4 中命名空间从 `Plugins.Anchor`（复数）变更为 `Plugin.Anchor`（单数）
- **Config 类移除**：不再使用 `AnchorManagerConfig` 和 `PrismPointConfig` 类，构造函数直接接收配置对象
- 必须在 `ViewAdded` 事件触发后才能创建锚点效果
- 锚点在模型场景中以棱锥形状悬浮显示，带上下浮动动画，常用于标记重要位置或设备

---

## 创建锚点管理器

```javascript
// v4 构造函数直接传入配置对象（无需 AnchorManagerConfig 类）
const anchorManager = new Glodon.Bimface.Plugin.Anchor.AnchorManager({
  viewer: viewer3D   // Viewer3D 实例
});
```

## 创建棱锥锚点（PrismPoint）

```javascript
// 直接传入配置对象（无需 PrismPointConfig 类）
const prismPoint = new Glodon.Bimface.Plugin.Anchor.PrismPoint({
  position: {                     // 锚点悬浮位置（世界坐标）
    x: 13907767.112090306,
    y: 26204006.755251624,
    z: 208873.42092020495
  },
  duration: 1500,                 // 悬浮动画循环一次的时间（毫秒）
  size: 35000                     // 锚点大小
});

// 将锚点添加到管理器
anchorManager.addItem(prismPoint);
```

## 更新锚点位置

```javascript
// 动态更新锚点的世界坐标
prismPoint.setPosition({
  x: 13908000.0,
  y: 26204500.0,
  z: 210000.0
});
anchorManager.update();
```

## 更新锚点大小

```javascript
// 调整锚点尺寸
prismPoint.setSize(50000);
anchorManager.update();
```

## 设置悬浮动画时长

```javascript
// 设置悬浮动画循环时间（毫秒）
prismPoint.setDuration(2000);
anchorManager.update();
```

## 显示/隐藏锚点

```javascript
// 显示锚点
prismPoint.show();

// 隐藏锚点
prismPoint.hide();
```

## 移除锚点

```javascript
// 从管理器中移除指定锚点
anchorManager.removeItem(prismPoint);

// 清空所有锚点
anchorManager.clear();
```

---

## 完整示例

```javascript
let anchorManager;

viewer3D.addEventListener(
  Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
  () => {
    // 1. 创建锚点管理器
    anchorManager = new Glodon.Bimface.Plugin.Anchor.AnchorManager({
      viewer: viewer3D
    });

    // 2. 创建多个棱锥锚点标记重要设备位置
    const point1 = new Glodon.Bimface.Plugin.Anchor.PrismPoint({
      position: { x: 13907767.11, y: 26204006.76, z: 208873.42 },
      duration: 1500,
      size: 35000
    });

    const point2 = new Glodon.Bimface.Plugin.Anchor.PrismPoint({
      position: { x: 13909000.00, y: 26205000.00, z: 210000.00 },
      duration: 2000,
      size: 40000
    });

    // 3. 批量添加锚点到管理器
    anchorManager.addItem(point1);
    anchorManager.addItem(point2);

    viewer3D.render();
  }
);
```

---

## 配置参数说明

### AnchorManager 构造参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| viewer | `Viewer3D` | 是 | 视图对象 |

### PrismPoint 构造参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| position | `{x, y, z}` | 是 | 锚点悬浮位置（世界坐标） |
| duration | `Number` | 否 | 悬浮动画循环时间（毫秒），默认 1500 |
| size | `Number` | 否 | 锚点大小 |

---

## 常用方法说明

| 方法 | 说明 |
|------|------|
| `new AnchorManager({viewer})` | 创建锚点管理器 |
| `anchorManager.addItem(prismPoint)` | 添加锚点到管理器 |
| `anchorManager.removeItem(prismPoint)` | 从管理器移除锚点 |
| `anchorManager.clear()` | 清空所有锚点 |
| `anchorManager.update()` | 刷新管理器渲染 |
| `new PrismPoint({position, duration, size})` | 创建棱锥锚点 |
| `prismPoint.setPosition({x, y, z})` | 更新锚点位置 |
| `prismPoint.setSize(size)` | 更新锚点大小 |
| `prismPoint.setDuration(ms)` | 更新动画时长 |
| `prismPoint.show()` | 显示锚点 |
| `prismPoint.hide()` | 隐藏锚点 |

---

## v3 → v4 对照

| 项目 | v3（旧版） | v4（新版） |
|------|-----------|-----------|
| 命名空间 | `Plugins.Anchor` | `Plugin.Anchor` |
| 管理器 Config | `new AnchorManagerConfig()` 设置属性 | 不再需要，直接传 `{viewer}` |
| 锚点 Config | `new PrismPointConfig()` 设置属性 | 不再需要，直接传 `{position, duration, size}` |
| 管理器构造 | `new AnchorManager(anchorMngConfig)` | `new AnchorManager({viewer})` |
| 锚点构造 | `new PrismPoint(prismPointConfig)` | `new PrismPoint({position, duration, size})` |
