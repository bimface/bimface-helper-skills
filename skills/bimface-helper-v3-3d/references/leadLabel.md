# 引线标签

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能添加引线标签
- 引线标签属于 `drawableContainer` 管理的标签之一，需确保标签容器已初始化（参照 [自定义标签](customLabel.md) 中的容器创建方式）
- 标签ID必须全局唯一，避免与其他标签冲突

## 添加引线标签

```javascript
// 引线标签的配置类
const config = new Glodon.Bimface.Plugins.Drawable.LeadLabelConfig();
// 引线标签的内容，需要根据用户实际的需求进行修改
config.text = "引线标签";
// 引线标签的三维世界坐标，需要根据用户点击的位置或输入的坐标信息进行修改
config.worldPosition = { x: 100, y: 200, z: 300 };
// 引线标签的视图
config.viewer = viewer3D;
// 引线标签的id，需要根据实际需求进行修改，避免重复
config.id = "leadLabel1";
// 初始化引线标签对象
let label = new Glodon.Bimface.Plugins.Drawable.LeadLabel(config);
// 添加引线标签到标签容器(drawableContainer)
drawableContainer.addItem(label);
```

## 移除引线标签

```javascript
// 根据id移除指定引线标签
drawableContainer.removeItemById("leadLabel1");
// 移除全部标签
drawableContainer.clear();
```
