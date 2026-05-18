# 聚合标签（Cluster）

## 使用约束与说明
- `ClusterContainer` 同时支持 `ViewerDrawing` 和 `Viewer3D`
- 聚合标签仅在 `ViewerDrawing` 下支持动画效果
- `distance` 属性仅在 `ViewerDrawing` 下可用，单位为像素

## 初始化聚合标签容器

```javascript
// 创建聚合标签容器配置
const clusterConfig = new Glodon.Bimface.Plugins.Cluster.ClusterContainerConfig();
clusterConfig.viewer = viewer2D;
clusterConfig.enableAnimation = true; // 仅2D支持动画效果

// 创建聚合标签容器
const clusterContainer = new Glodon.Bimface.Plugins.Cluster.ClusterContainer(clusterConfig);
```

## 创建聚合标签

```javascript
// 创建聚合标签配置
const itemConfig = new Glodon.Bimface.Plugins.Cluster.ClusterItemConfig();
itemConfig.tags = [tag1, tag2, tag3]; // 需要被聚合的标签列表
itemConfig.distance = 50;             // 聚合半径范围（px），仅ViewerDrawing可用，默认50
itemConfig.maxLevel = 4;             // 最大缩放层级，0-10，默认4
itemConfig.minClusterSize = 2;       // 最小聚合个数，默认2
itemConfig.scale = 1;                // 聚合标签大小缩放值，默认1
itemConfig.showDetails = false;      // 聚合数大于99时是否显示具体数值

// 创建聚合标签并添加到容器
const clusterItem = new Glodon.Bimface.Plugins.Cluster.ClusterItem(itemConfig);
clusterContainer.addItem(clusterItem);
```

## 聚合标签样式

```javascript
// 通过style对象设置聚合标签样式，支持四种预设样式
const style = new Glodon.Bimface.Plugins.Cluster.ClusterStyle();
// 样式类型: Success / Warning / Information / Danger
itemConfig.style = style;
```
