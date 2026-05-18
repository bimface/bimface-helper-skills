# 设置三维标签

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能创建三维标签容器
- 三维标签的点击事件回调应使用具名函数（参照 [监听事件](events.md) 规范）
- 通过 `MouseClicked` 事件获取坐标时需做防御性判断（参照 [监听事件](events.md) 中鼠标点击事件）

## 创建三维标签容器

```JavaScript
//  创建一个用于管理所有三维标签的容器，先创建它的config对象，然后再初始化容器
const markerContainerConfig = new Glodon.Bimface.Plugins.Marker3D.Marker3DContainerConfig();
markerContainerConfig.viewer = viewer3D;  // 必须设置viewer，只适用于Viewer3D
const markerContainer = new Glodon.Bimface.Plugins.Marker3D.Marker3DContainer(markerContainerConfig);
```

## 创建三维标签对象

```JavaScript
// 创建一个三维标签对象，先创建它的config对象，然后再初始化标签
const markerConfig = new Glodon.Bimface.Plugins.Marker3D.Marker3DConfig();
markerConfig.canvas = canvas;  // 为标签添加canvas内容
markerConfig.size = 100;  // 设置标签的大小，单位是像素
markerConfig.modelId = modelId;  // 可设置模型ID
markerConfig.objectId = objectId;  // 可选设置构件ID
markerConfig.worldPosition = position;  // 必须设置位置，是一个三维向量（如{x: 100, y: 200, z: 300}），这个坐标一般由开发者指定，或者是通过Viewer3DEvent中的MouseClicked事件获取
markerConfig.tooltip = "This is 3DMarker.";  // 标签的提示内容，在鼠标悬浮在其上方时出现，可以不设置
const marker = new Glodon.Bimface.Plugins.Marker3D.Marker3D(markerConfig);  // 根据config创建三维标签对象
marker.onClick(clickFunction);  // 为标签添加点击事件，点击时会调用clickFunction函数。注意事件监听方法（如 onClick 、 addEventListener ）应直接绑定到具体对象上，不要放到容器上
markerContainer.addItem(marker);  // 将标签添加到容器中，就可以在场景中看到这个标签了
```

三维标签对象包含以下属性：
- id: 标签的唯一标识符，在创建标签时会自动生成一个随机的id，后续在容器对象中需要根据这个ID来显隐、移除标签
- position: 标签的位置，是一个三维向量（如{x: 100, y: 200, z: 300}），表示标签在场景中的坐标位置
- modelId: 标签所属的模型ID
- objectId: 标签所属的构件ID

## 通过容器控制三维标签对象

```JavaScript
// 获取容器中的所有三维标签对象
const markers = markerContainer.getAllItems();
// 获取指定ID的三维标签对象
const marker = markerContainer.getItemById(id);
// 移除所有容器内的三维标签对象
markerContainer.clear();
// 移除指定ID的三维标签对象
markerContainer.removeItemById(id);
// 显示指定ID的三维标签对象，这里的ids是一个数组，包含多个标签的ID
markerContainer.showItemsById(ids);
// 隐藏指定ID的三维标签对象，这里的ids是一个数组，包含多个标签的ID
markerContainer.hideItemsById(ids);
// 显示所有容器内的三维标签对象
markerContainer.showAllItems();
// 隐藏所有容器内的三维标签对象
markerContainer.hideAllItems();
// 重新渲染容器内的所有三维标签对象
markerContainer.update();
```
