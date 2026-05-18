# 显示模式与视图切换

## 使用约束与说明
- 必须在图纸 `Loaded` 事件触发后才能进行操作
- 显示模式切换后需调用 `viewer2D.render()` 刷新

## 切换显示模式

```javascript
// 设置显示模式
// 0: 普通模式（图元颜色与dwg源文件一致，背景为黑色）
// 1: 白底模式（图元颜色不变，背景为白色）
// 2: 黑白模式（图元颜色皆为黑色，背景为黑色）
viewer2D.setDisplayMode(2);
viewer2D.render();
```

## 获取当前显示模式

```javascript
// 返回值: 0=普通模式, 1=白底模式, 2=黑白模式
const currentMode = viewer2D.getDisplayMode();
```

## 自定义显示模式

```javascript
// 先切换到自定义模式（模式3）
viewer2D.setDisplayMode(3);

// 设置自定义背景颜色
const bgColor = new Glodon.Web.Graphics.Color("#FFFFFF", 1);
viewer2D.setBackgroundColor(bgColor);

// 恢复默认背景颜色
viewer2D.restoreBackgroundColor();
```

## 获取所有视图信息

```javascript
// 获取指定图纸的所有视图信息（Model视图和Layout视图）
const views = viewer2D.getViews(modelId);
// views是一个数组，每个元素包含视图的name和ID
```

## 切换到指定视图

```javascript
// 根据视图ID显示指定的视图
viewer2D.showViewById(viewId);
viewer2D.render();
```

## 获取当前视图ID

```javascript
const currentViewId = viewer2D.getCurrentViewId();
```

## 以源文件视图状态打开

```javascript
viewer2D.enableViewport(true);
```
