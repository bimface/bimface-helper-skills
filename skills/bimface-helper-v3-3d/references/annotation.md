# 模型批注

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能初始化批注工具条
- `createSnapshot` 是异步回调，快照结果需在回调函数中处理
- 批注事件监听器必须使用具名函数（参照 [监听事件](events.md) 规范）

## 初始化批注工具条

```javascript
// 创建批注工具条的配置
const config = new Glodon.Bimface.Plugins.Annotation.AnnotationToolbarConfig();
config.viewer = viewer3D;

// 创建批注工具条
const annotationToolbar = new Glodon.Bimface.Plugins.Annotation.AnnotationToolbar(config);

// 设置批注填充颜色
annotationToolbar.getAnnotationManager().setFillColor(new Glodon.Web.Graphics.Color(255, 0, 0, 0.3));

// 注册批注保存事件监听
annotationToolbar.addEventListener(Glodon.Bimface.Plugins.Annotation.AnnotationToolbarEvent.Saved, onAnnotationSaved);

// 注册批注取消事件监听
annotationToolbar.addEventListener(Glodon.Bimface.Plugins.Annotation.AnnotationToolbarEvent.Cancelled, onAnnotationCancelled);
```

## 开始绘制批注

```javascript
// 隐藏主工具条，避免和批注工具条重叠冲突
app3D.getToolbar("MainToolbar").hide();

// 显示批注工具条
annotationToolbar.show();
```

## 退出批注

```javascript
// 退出批注编辑模式
annotationToolbar.getAnnotationManager().exit();

// 显示主工具条
app3D.getToolbar("MainToolbar").show();
```

## 保存批注

```javascript
// 获取当前批注状态
const annotationState = annotationToolbar.getAnnotationManager().getCurrentState();
console.log(JSON.stringify(annotationState));

// 导出批注快照
annotationToolbar.getAnnotationManager().createSnapshot(function (img) {
    const image = new Image();
    image.src = img;
    document.body.appendChild(image);
});
```

## 恢复批注

```javascript
// 恢复批注状态
annotationToolbar.getAnnotationManager().setState(annotationState);

// 开始绘制批注
drawAnnotation();
```

## 启用/禁用批注选择

```javascript
// 禁用批注选择
annotationToolbar.getAnnotationManager().enablePick(false);

// 启用批注选择
annotationToolbar.getAnnotationManager().enablePick(true);
```

## 启用/禁用批注移动

```javascript
// 禁止批注移动（只允许删除、编辑、旋转、拉伸）
annotationToolbar.getAnnotationManager().setOperationMode(["Delete", "Edit", "Rotate", "Stretch"]);

// 允许批注移动
annotationToolbar.getAnnotationManager().setOperationMode(["Delete", "Edit", "Move", "Rotate", "Stretch"]);
```

## 监听批注绘制完成事件

```javascript
// 添加批注绘制完成事件监听
annotationToolbar.getAnnotationManager().itemCompleted(function (data) {
    const type = data.markupType; // 获取批注类型
    console.log(type);
});
```

## 启用/禁用捕捉模式

```javascript
// 构造捕捉模式
const snapMode = new Glodon.Bimface.Viewer.SnapMode();

// 设置捕捉对象：Endpoint（端点）、Line（线）、Face（面）
const endpoint = Glodon.Bimface.Viewer.SnapObject.Endpoint;
const line = Glodon.Bimface.Viewer.SnapObject.Line;
const face = Glodon.Bimface.Viewer.SnapObject.Face;
snapMode.setSnap3DList([endpoint, line, face]);

// 应用捕捉设置
viewer3D.setSnapMode(snapMode);

// 启用捕捉模式
viewer3D.enableSnap(true);

// 关闭捕捉模式
viewer3D.enableSnap(false);
```
