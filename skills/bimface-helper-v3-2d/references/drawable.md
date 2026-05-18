# 二维标签（Drawable）

## 使用约束与说明
- `DrawableContainer` 同时支持 `ViewerDrawing` 和 `Viewer3D`
- 图纸中 worldPosition 为二维坐标格式 `{x, y}`（无z轴）
- 标签需在图纸 `Loaded` 事件后才能添加
- `modelId` / `objectId` / `enableDepthTest` / `visibleDistance` 等属性仅Viewer3D有效，图纸场景下无需设置

## 初始化标签容器

```javascript
// 创建标签容器配置
const drawableConfig = new Glodon.Bimface.Plugins.Drawable.DrawableContainerConfig();
drawableConfig.viewer = viewer2D;

// 创建标签容器
const drawableContainer = new Glodon.Bimface.Plugins.Drawable.DrawableContainer(drawableConfig);
```

## 自定义标签（CustomItem）

```javascript
// 创建自定义标签配置
const customConfig = new Glodon.Bimface.Plugins.Drawable.CustomItemConfig();
customConfig.id = 'customLabel_1';
customConfig.worldPosition = { x: 100, y: 200 };
customConfig.opacity = 1;
customConfig.viewer = viewer2D;     // 关联的viewer对象
customConfig.draggable = true;      // 是否可拖拽
customConfig.offsetY = -32;         // Y方向偏移量（px），正值向下

// 设置标签内容（支持任意HTML DOM元素）
const content = document.createElement('div');
content.style.cssText = 'width: 100px; height: 32px; background: #FF6600;'
    + 'color: #FFFFFF; text-align: center; line-height: 32px; border-radius: 4px;';
content.innerText = '检查点';
customConfig.content = content;

// 创建标签对象并添加到容器
const customItem = new Glodon.Bimface.Plugins.Drawable.CustomItem(customConfig);
drawableContainer.addItem(customItem);
```

## 引线标签（LeadLabel）

```javascript
const leadConfig = new Glodon.Bimface.Plugins.Drawable.LeadLabelConfig();
leadConfig.id = 'leadLabel1';
leadConfig.text = '引线标签';
leadConfig.worldPosition = { x: 100, y: 200 };
leadConfig.viewer = viewer2D;

const leadLabel = new Glodon.Bimface.Plugins.Drawable.LeadLabel(leadConfig);
drawableContainer.addItem(leadLabel);
```

## 图片标签（Image）

```javascript
// 创建图片标签配置
const imageConfig = new Glodon.Bimface.Plugins.Drawable.ImageConfig();
imageConfig.id = 'image_1';
imageConfig.src = 'https://example.com/image.png';
imageConfig.worldPosition = { x: 100, y: 200 };
imageConfig.width = 32;
imageConfig.height = 32;
imageConfig.opacity = 1;

// 设置点击回调
const image = new Glodon.Bimface.Plugins.Drawable.Image(imageConfig);
image.onClick(function(data) {
    console.log("图片被点击", data);
});
drawableContainer.addItem(image);
```

## 标签容器操作

```javascript
// 获取所有标签
const allItems = drawableContainer.getAllItems();

// 按ID获取标签
const item = drawableContainer.getItemById('customLabel_1');

// 按ID移除标签
drawableContainer.removeItemById('customLabel_1');

// 按ID列表显示标签
drawableContainer.showItemsById(['customLabel_1', 'leadLabel1']);

// 按ID列表隐藏标签
drawableContainer.hideItemsById(['customLabel_1', 'leadLabel1']);

// 显示所有标签
drawableContainer.showAllItems();

// 隐藏所有标签
drawableContainer.hideAllItems();

// 更新所有标签（重新渲染）
drawableContainer.update();

// 清除所有标签
drawableContainer.clear();
```

## 标签事件

```javascript
// 左键点击
customItem.onClick(function() {
    console.log("左键点击");
});

// 右键点击
customItem.onRightClick(function() {
    console.log("右键点击");
});

// 拖拽结束（返回当前位置）
customItem.onEndDrag(function(data) {
    console.log("当前位置:", data.worldPosition);
});
```
