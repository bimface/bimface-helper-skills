# 聚合标签

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能初始化聚合标签容器
- 聚合标签的数据源来自 `drawableContainer`，需确保标签容器已创建并添加了标签
- 事件监听器应使用具名函数（参照 [监听事件](events.md) 规范）

## 创建聚合标签容器

```javascript
// 构造聚合标签容器配置项
const clusterContainerConfig = new Glodon.Bimface.Plugins.Cluster.ClusterContainerConfig();
clusterContainerConfig.viewer = viewer3D;

// 构造聚合标签容器
const clusterContainer = new Glodon.Bimface.Plugins.Cluster.ClusterContainer(clusterContainerConfig);
```

## 初始化聚合标签

```javascript
// 构造聚合标签配置项
const clusterItemConfig = new Glodon.Bimface.Plugins.Cluster.ClusterItemConfig();

// 设置需要聚合的标签列表（从DrawableContainer中获取）
clusterItemConfig.tags = drawableContainer.getAllItems();

// 设置最大聚合级别
clusterItemConfig.maxLevel = 8;

// 关联viewer
clusterItemConfig.viewer = viewer3D;

// 构造聚合标签对象
const clusterItem = new Glodon.Bimface.Plugins.Cluster.ClusterItem(clusterItemConfig);
```

## 设置标签点击事件

```javascript
// 定义聚合标签的点击事件
clusterItem.onClick(function (data) {
    const boundingBox = data.boundingBox;
    // 缩放到点击的聚合标签区域
    viewer3D.getCamera().zoomToBoundingBox({ boundingBox: boundingBox, margin: 5 });
});
```

## 添加聚合标签到容器

```javascript
// 将聚合标签添加到容器
clusterContainer.addCluster(clusterItem);

// 更新容器
clusterContainer.update();
```

## 设置标签样式（状态）

```javascript
// 设置标签为危险样式（红色）
clusterItem.setException(tagId, Glodon.Bimface.Plugins.Cluster.ClusterStyle.Danger);

// 设置标签为警告样式（黄色）
clusterItem.setException(tagId, Glodon.Bimface.Plugins.Cluster.ClusterStyle.Warning);

// 设置标签为信息样式（蓝色）
clusterItem.setException(tagId, Glodon.Bimface.Plugins.Cluster.ClusterStyle.Information);

// 设置标签为成功样式（绿色，默认）
clusterItem.setException(tagId, Glodon.Bimface.Plugins.Cluster.ClusterStyle.Success);

// 更新聚合标签状态
clusterItem.updateClusterTags();
```

## 清空标签样式

```javascript
// 重置所有标签样式为默认（Success）
clusterItem.clearException();

// 更新聚合标签状态
clusterItem.updateClusterTags();
```

## 根据距离设置标签大小

```javascript
// 根据相机到场景中心的距离设置聚合标签的大小
const onCameraChanged = function (data) {
    if (data.far <= 300000) {
        clusterItem.setScale(1.00);
    } else if (data.far > 300000 && data.far <= 500000) {
        clusterItem.setScale(0.85);
    } else if (data.far > 500000 && data.far <= 700000) {
        clusterItem.setScale(0.70);
    } else if (data.far > 700000 && data.far <= 900000) {
        clusterItem.setScale(0.55);
    } else {
        clusterItem.setScale(0.40);
    }
};

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.CameraPositionChanged, onCameraChanged);
```

## 清空聚合标签

```javascript
// 清空容器中所有聚合标签
clusterContainer.clear();
// 更新容器
clusterContainer.update();
```
