# 导航地图（NavigationMap）

## 使用约束与说明

- NavigationMap 由 `Plugin.Navigation.NavigationMap` 构造函数直接创建（非 Widget 模式）
- 需要传入 DOM 元素作为地图渲染容器
- 支持两种数据来源：`SectionSnapshot`（模型剖切快照）和 `CustomImage`（自定义图片）
- 必须在模型加载完成后创建
- 命名空间为 `Plugin`（单数）

---

## 创建 NavigationMap

### 构造函数

```javascript
new Glodon.Bimface.Plugin.Navigation.NavigationMap({
  viewer: viewer3D,       // Viewer3D 实例
  domElement: domElement, // 放置地图的 DOM 元素
  source: sourceConfig    // 数据源配置
});
```

---

## 数据源类型

### 1. SectionSnapshot —— 剖切快照

在指定高度对模型进行剖切，生成楼层平面图作为导航地图：

```javascript
const source = {
  type: 'SectionSnapshot',
  height: 6  // 剖切高度（模型坐标系 Z 轴）
};
```

### 2. CustomImage —— 自定义图片

使用自定义图片作为导航地图，需要提供图片 URL 和锚点对应关系：

```javascript
const source = {
  type: 'CustomImage',
  url: './floor-plan.png',           // 图片地址
  anchors: {
    map: [                           // 地图上的锚点坐标（2D）
      { x: 100, y: 200 },
      { x: 500, y: 600 }
    ],
    model: [                         // 模型中对应的锚点坐标（3D）
      { x: 0, y: 10, z: 0 },
      { x: 50, y: 10, z: 40 }
    ]
  }
};
```

> 锚点用于将 2D 地图坐标与 3D 模型坐标建立映射关系，至少需要提供 2 组对应点。

---

## destroy —— 销毁地图

```javascript
navigationMap.destroy();
```

---

## 完整示例

### 示例 1：剖切快照方式

```javascript
const viewer3D = new Glodon.Bimface.Viewer.Viewer3D(viewer3DConfig);
viewer3D.addModel(viewToken);

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelLoaded, () => {
  // 获取用于承载地图的 DOM 元素
  const mapContainer = document.getElementById("navigationMapContainer");

  // 使用剖切快照方式创建导航地图（在高度 6 处剖切）
  const navigationMap = new Glodon.Bimface.Plugin.Navigation.NavigationMap({
    viewer: viewer3D,
    domElement: mapContainer,
    source: {
      type: 'SectionSnapshot',
      height: 6
    }
  });

  // 不再需要时销毁
  // navigationMap.destroy();
});
```

### 示例 2：自定义图片方式

```javascript
const viewer3D = new Glodon.Bimface.Viewer.Viewer3D(viewer3DConfig);
viewer3D.addModel(viewToken);

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelLoaded, () => {
  const mapContainer = document.getElementById("navigationMapContainer");

  // 使用自定义图片创建导航地图
  const navigationMap = new Glodon.Bimface.Plugin.Navigation.NavigationMap({
    viewer: viewer3D,
    domElement: mapContainer,
    source: {
      type: 'CustomImage',
      url: './building-floor-plan.png',
      anchors: {
        map: [
          { x: 0, y: 0 },
          { x: 800, y: 600 }
        ],
        model: [
          { x: -40, y: 5, z: -30 },
          { x: 40, y: 5, z: 30 }
        ]
      }
    }
  });

  // 切换地图的显示/隐藏
  document.getElementById("btnToggleMap").addEventListener("click", () => {
    if (mapContainer.style.display === "none") {
      mapContainer.style.display = "block";
    } else {
      mapContainer.style.display = "none";
    }
  });

  // 销毁地图
  document.getElementById("btnDestroyMap").addEventListener("click", () => {
    navigationMap.destroy();
  });
});
```

---

## 注意事项

- DOM 容器元素需要提前设置好尺寸（宽高），否则地图可能无法正确渲染
- `SectionSnapshot` 方式适合 BIM 模型，自动生成楼层平面
- `CustomImage` 方式适合已有平面图的场景，需要精确的锚点映射以确保 2D/3D 位置对应
- 锚点数量建议 ≥ 2 组，且尽量覆盖地图的主要区域以提高映射精度
- 调用 `destroy()` 后，如需重新创建地图需再次 new 实例
