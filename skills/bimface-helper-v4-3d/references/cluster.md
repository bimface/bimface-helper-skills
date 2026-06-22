# 聚合标签（Cluster）

## 使用约束与说明

- 聚合标签依赖 Marker3D，需先理解 Marker3DItem 的创建和配置（参见 [marker3D.md](marker3D.md)）
- Cluster 由两个类组成：`ClusterItem`（聚合数据项）和 `ClusterContainer`（聚合容器）
- 命名空间为 `Plugin.Cluster`（单数 Plugin）
- 聚合效果：当相机拉远时，相邻的 3D 标签会自动聚合成一个组标签，拉近时拆分为独立标签
- `maxLevel` 控制最大聚合层级，值越大聚合粒度越粗
- 必须在场景加载完成（`ViewAdded` 或 `ModelLoaded` 事件）后使用

---

## 创建 ClusterItem

```javascript
// tags: Marker3DItem 数组，需要聚合的标签集合
// maxLevel: 最大聚合层级
const clusterItem = new Glodon.Bimface.Plugin.Cluster.ClusterItem({
  viewer: viewer3D,
  tags: markerItems,
  maxLevel: 3
});
```

| 参数 | 类型 | 说明 |
|------|------|------|
| viewer | Viewer3D | 查看器实例 |
| tags | Marker3DItem[] | 要参与聚合的标签数组 |
| maxLevel | Number | 最大聚合层级，值越大聚合程度越高 |

---

## 创建 ClusterContainer

```javascript
const clusterContainer = new Glodon.Bimface.Plugin.Cluster.ClusterContainer({
  viewer: viewer3D
});
```

---

## addItems —— 添加聚合项

```javascript
// 将一个或多个 ClusterItem 添加到容器中
clusterContainer.addItems([clusterItem1, clusterItem2]);
```

---

## 完整示例

```javascript
const viewer3D = new Glodon.Bimface.Viewer.Viewer3D(viewer3DConfig);
viewer3D.addModel(viewToken);
const model = viewer3D.getModel();

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelLoaded, async () => {
  // 1. 获取模型包围盒，用于随机生成标签位置
  const boundingBox = await model.getBoundingBox();
  const { min, max } = boundingBox;

  // 辅助函数：生成范围内的随机坐标
  const randomPos = () => ({
    x: min.x + Math.random() * (max.x - min.x),
    y: min.y + Math.random() * (max.y - min.y),
    z: min.z + Math.random() * (max.z - min.z)
  });

  // 2. 创建 Marker3DItem 数组
  const iconUrl = "./marker-icon.png";
  const markerItems = [];

  for (let i = 0; i < 50; i++) {
    const marker = new Glodon.Bimface.Plugin.Marker3D.Marker3DItem({
      id: `marker-${i}`,
      worldPosition: randomPos(),
      visualization: {
        url: iconUrl,
        size: 20,
        hoverAnimation: true,
        tooltip: `设备 #${i + 1}`
      }
    });

    // 为每个标签注册点击事件
    marker.addEventListener(
      Glodon.Bimface.Plugin.Marker3D.Marker3DEvent.Click,
      (data) => {
        if (data.eventType === "Click") {
          console.log("点击了标签:", data.id, "位置:", data.position);
        }
      }
    );

    markerItems.push(marker);
  }

  // 3. 创建 ClusterItem（将被聚合的标签打包）
  const clusterItem = new Glodon.Bimface.Plugin.Cluster.ClusterItem({
    viewer: viewer3D,
    tags: markerItems,
    maxLevel: 3          // 最大聚合层级
  });

  // 4. 创建 ClusterContainer 并将 ClusterItem 加入
  const clusterContainer = new Glodon.Bimface.Plugin.Cluster.ClusterContainer({
    viewer: viewer3D
  });

  clusterContainer.addItems([clusterItem]);

  console.log(`已创建聚合标签：${markerItems.length} 个标签，最大聚合层级 ${3}`);
});
```

### 多组聚合示例

```javascript
viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelLoaded, async () => {
  // 第一组：暖通设备标签（聚合层级低，更容易拆分）
  const hvacMarkers = createMarkers("HVAC", 30);
  const hvacCluster = new Glodon.Bimface.Plugin.Cluster.ClusterItem({
    viewer: viewer3D,
    tags: hvacMarkers,
    maxLevel: 2
  });

  // 第二组：给排水设备标签（聚合层级高，更容易聚合在一起）
  const plumbingMarkers = createMarkers("PLUMBING", 20);
  const plumbingCluster = new Glodon.Bimface.Plugin.Cluster.ClusterItem({
    viewer: viewer3D,
    tags: plumbingMarkers,
    maxLevel: 4
  });

  // 将两组聚合项加入同一容器
  const clusterContainer = new Glodon.Bimface.Plugin.Cluster.ClusterContainer({
    viewer: viewer3D
  });

  clusterContainer.addItems([hvacCluster, plumbingCluster]);
});
```

---

## 注意事项

- 聚合标签需要与 Marker3D 配合使用，Marker3DItem 的创建参考 [marker3D.md](marker3D.md)
- `maxLevel` 越大，标签越容易被聚合（即拉近时才拆分）；`maxLevel` 越小则越容易保持独立
- 每个 `ClusterItem` 内包含一组 `tags`，同一组内的标签之间才会互相聚合
- 不同 `ClusterItem` 之间的标签不会互相聚合
- 聚合效果会随相机距离自动调整，无需手动触发
