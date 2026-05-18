# 设置自定义标签

## 使用约束与说明
- 一个场景通常只需要一个标签容器。标签的添加、删除、显隐操作，必须基于全局初始化的drawableContainer对象
- 在监听事件以初始化容器或添加标签时，需遵守events.md规范，禁止使用箭头函数导致事件无法被注销
- 通过点击事件获取三维坐标时，必须进行防御性校验，避免因点击空白背景导致报错

## 使用步骤
在BIMFACE中，添加一些自定义的二维标签。由于这些标签本质上是一个HTML元素，所以可以通过CSS样式来自定义其外观，表达任何你想要的信息。标签创建步骤如下：
- 首先需要一个用于管理所有标签的容器（Container），一般来说一个场景只需要一个容器即可。
- 然后，需要创建一个标签对象，在其中定义它的位置、内容、样式、事件等，当然内容和样式的写法与常规的前端HTML元素相同
- 最后，将这个标签对象添加到容器中，就可以在场景中看到这个标签了
这个标签可以被用在模型（Viewer3D）或图纸（ViewerDrawing）中。

## 创建标签容器
```JavaScript
// 一般需要在场景加载完成后初始化容器，或者是在明确的一个用户触发的事件后（例如明确要创建第一个标签的时候）初始化容器

// 定义全局标签容器变量
let drawableContainer;

// 场景加载完成事件处理函数
const initDrawableContainer = function(eventData) {
  // 创建一个用于管理所有自定义标签的容器，先创建它的config对象，然后再初始化容器
  let drawableConfig = new Glodon.Bimface.Plugins.Drawable.DrawableContainerConfig();
  drawableConfig.viewer = viewer3D;  // 设置标签容器要关联场景，这里赋值一个Viewer3D对象
  drawableContainer = new Glodon.Bimface.Plugins.Drawable.DrawableContainer(drawableConfig);
}

// 关于viewer3D下的事件内容，可参考reference中的events.md
viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded, initDrawableContainer)
```

## 创建自定义标签
```JavaScript
function addItem(position) {
  if (!drawableContainer) {
    console.error("标签容器尚未初始化！");
    return;
  }

  // 构造自定义标签配置项
  let config = new Glodon.Bimface.Plugins.Drawable.CustomItemConfig();
  // 定义自定义标签的内容，这里的content是一个HTML元素，所以可以根据需要自定义它的内容与样式
  let content = document.createElement('div');
  content.style.width = '100px';
  content.style.height = '32px';
  content.style.border = 'solid';
  content.style.background = '#32D3A6';
  content.innerHTML = '自定义标签';
  content.style.color = '#FFFFFF';
  content.style.lineHeight = '32px';
  // 设置自定义标签的内容
  config.content = content;
  // 设置自定义标签的不透明度
  config.opacity = 1;
  // 设置自定义标签的坐标
  config.worldPosition = position;
  // 设置自定义标签的ID
  config.id = 'customLabel_1';
  // 构造自定义标签对象
  const customItem = new Glodon.Bimface.Plugins.Drawable.CustomItem(config);
  // 将自定义标签添加到标签容器中
  drawableContainer.addItem(customItem);
}
```
需要注意的是，自定义标签的坐标（worldPosition）是一个三维向量，可以直接设置一个场景中的坐标值（{x: 123, y: 456, z: 789}），也可以根据点击事件获取，例如：
```JavaScript
const onMapClickAddLabel = function(eventData) {
    // 防御性判断：确保点中了模型，获取到了有效的三维坐标
    if (!eventData.worldPosition) {
        return;
    }
    // 将点击事件获取的场景坐标作为参数传入
    addItem(eventData.worldPosition);
};

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.MouseClicked, onMapClickAddLabel)
```
自定义标签对象包含以下属性：
- id: 标签的唯一标识符，在创建标签时会自动生成一个随机的id，后续在容器对象中需要根据这个ID来显隐、移除标签
- position: 标签的位置，是一个三维向量（如{x: 100, y: 200, z: 300}），表示标签在场景中的坐标位置
- modelId: 标签所属的模型ID
- objectId: 标签所属的构件ID

## 给自定义标签添加点击事件

CustomItem 的 content 是 HTML 元素，直接在 DOM 元素上绑定事件即可：

```JavaScript
function addClickableLabel(position, componentName) {
  if (!drawableContainer) {
    console.error("标签容器尚未初始化！");
    return;
  }

  let config = new Glodon.Bimface.Plugins.Drawable.CustomItemConfig();

  let content = document.createElement('div');
  content.style.padding = '6px 14px';
  content.style.background = '#32D3A6';
  content.style.color = '#FFFFFF';
  content.style.borderRadius = '6px';
  content.style.fontSize = '13px';
  content.style.fontWeight = '600';
  content.style.cursor = 'pointer';
  content.style.userSelect = 'none';
  content.textContent = componentName;

  // 直接在 DOM 元素上绑定点击事件
  content.addEventListener('click', function (e) {
    e.stopPropagation();
    console.log('标签被点击:', componentName);
    // 在这里处理点击逻辑，如打开详情面板
  });

  config.content = content;
  config.worldPosition = position;
  config.id = 'label_' + Date.now();
  config.opacity = 1;

  const customItem = new Glodon.Bimface.Plugins.Drawable.CustomItem(config);
  drawableContainer.addItem(customItem);
}
```

## 通过容器控制自定义标签对象
标签对象的核心属性包含ID、position、objectId等。可通过drawableContainer对其进行批量或单独的管控
```JavaScript
const targetId = 'customLabel_1';
const targetIds = ['customLabel_1', 'customLabel_2'];
// 获取容器中的所有自定义标签对象
const markers = drawableContainer.getAllItems();
// 获取指定ID的自定义标签对象
const marker = drawableContainer.getItemById(targetId);

// 移除指定ID的自定义标签对象
drawableContainer.removeItemById(targetId);
// 显示指定ID的自定义标签对象，这里的ids是一个数组，包含多个标签的ID
drawableContainer.showItemsById(targetIds);
// 隐藏指定ID的自定义标签对象，这里的ids是一个数组，包含多个标签的ID
drawableContainer.hideItemsById(targetIds);
// 显示所有容器内的自定义标签对象
drawableContainer.showAllItems();
// 隐藏所有容器内的自定义标签对象
drawableContainer.hideAllItems();
// 重新渲染容器内的所有自定义标签对象
drawableContainer.update();
// 移除所有容器内的自定义标签对象
drawableContainer.clear();
```