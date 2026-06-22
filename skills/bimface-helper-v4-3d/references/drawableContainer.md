# 自定义绘制容器（DrawableContainer）

## 使用约束与说明

- **命名空间变更**：v4 中命名空间从 `Plugins.Drawable`（复数）变更为 `Plugin.Drawable`（单数）
- **Config 类移除**：不再使用 `DrawableContainerConfig` 类，构造函数直接接收配置对象
- `DrawableContainer` 是 3D 场景中承载自定义绘制对象（如 `CustomItem`）的容器
- 必须在场景加载完成（触发 `ViewAdded` 事件）后才能初始化
- 具体绘制对象的创建和使用请参考 `customItem.md`

## 初始化 DrawableContainer

```javascript
let drawableContainer;

function initDrawableContainer() {
  if (drawableContainer) {
    return;
  }

  // 直接传入配置对象（无需 DrawableContainerConfig 类）
  drawableContainer = new Glodon.Bimface.Plugin.Drawable.DrawableContainer({
    viewer: viewer3D    // viewer3D 为 Viewer3D 实例
  });

  console.log('DrawableContainer 初始化完成');
}
```

## 基本使用模式

`DrawableContainer` 本身是容器，配合具体的绘制对象（如 `CustomItem`）使用。以下是基本使用模式：

```javascript
// 1. 初始化容器
const drawableContainer = new Glodon.Bimface.Plugin.Drawable.DrawableContainer({
  viewer: viewer3D
});

// 2. 创建具体的绘制对象（以 CustomItem 为例）
// 详细用法请参考 customItem.md
// const customItem = new Glodon.Bimface.Plugin.Drawable.CustomItem({...});

// 3. 将绘制对象添加到容器中
// drawableContainer.addItem(customItem);

// 4. 场景渲染
viewer3D.render();
```

## v3 → v4 对照

| 项目 | v3（旧版） | v4（新版） |
|------|-----------|-----------|
| 命名空间 | `Plugins.Drawable` | `Plugin.Drawable` |
| 构造 | `new DrawableContainer(new DrawableContainerConfig())` | `new DrawableContainer({viewer})` |
| Config 类 | `DrawableContainerConfig` | 不再需要，直接传对象 |
