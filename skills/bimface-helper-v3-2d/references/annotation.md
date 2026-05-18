# 图纸批注

## 使用约束与说明
- 必须在图纸 `Loaded` 事件触发后才能创建批注工具条
- 批注一旦开启绘制，图纸视角将被固定，直到退出批注
- 批注工具条激活时会隐藏主工具条，退出批注后需恢复主工具条显示
- 批注内容需通过 `getCurrentState()` 保存、`setState()` 恢复

## 创建批注工具条

```javascript
let isDrawAnnotationActivated = false;
let annotationToolbar = null;
let annotationState = null;

function createAnnotationToolbar() {
    if (!annotationToolbar) {
        let config = new Glodon.Bimface.Plugins.Annotation.AnnotationToolbarConfig();
        config.viewer = viewer2D;
        annotationToolbar = new Glodon.Bimface.Plugins.Annotation.AnnotationToolbar(config);
        // 设置填充颜色
        annotationToolbar.getAnnotationManager().setFillColor(
            new Glodon.Web.Graphics.Color(255, 0, 0, 0.3)
        );
        // 注册批注工具条事件
        annotationToolbar.addEventListener(
            Glodon.Bimface.Plugins.Annotation.AnnotationToolbarEvent.Saved,
            onAnnotationSaved
        );
        annotationToolbar.addEventListener(
            Glodon.Bimface.Plugins.Annotation.AnnotationToolbarEvent.Cancelled,
            exitAnnotation
        );
    }
}
```

## 开始绘制批注

```javascript
function drawAnnotation() {
    createAnnotationToolbar();
    if (!isDrawAnnotationActivated) {
        app2D.getToolbar("MainToolbar").hide();  // 隐藏主工具条
        annotationToolbar.show();                 // 显示批注工具条
        isDrawAnnotationActivated = true;
    }
}
```

## 保存批注并退出

```javascript
function onAnnotationSaved() {
    // 保存批注状态，用于后续恢复
    annotationState = annotationToolbar.getAnnotationManager().getCurrentState();
    // 导出批注快照（BASE64图片）
    annotationToolbar.getAnnotationManager().createSnapshot(function(img) {
        const image = new Image();
        image.src = img;
        document.body.appendChild(image);
    });
    exitAnnotation();
}
```

## 退出批注

```javascript
function exitAnnotation() {
    app2D.getToolbar("MainToolbar").show();        // 恢复主工具条
    annotationToolbar.getAnnotationManager().exit(); // 退出批注管理器
    isDrawAnnotationActivated = false;
}
```

## 恢复批注

```javascript
function restoreAnnotation() {
    if (annotationState != null) {
        annotationToolbar.getAnnotationManager().setState(annotationState);
        drawAnnotation();
    }
}
```

## 控制批注交互

```javascript
// 禁用/启用批注图元的选择
annotationToolbar.getAnnotationManager().enablePick(false); // 禁止选择
annotationToolbar.getAnnotationManager().enablePick(true);  // 允许选择

// 设置批注操作模式，[] 表示不支持
// "Delete" 删除 / "Edit" 编辑内容(仅文字) / "Move" 移动 / "Rotate" 旋转 / "Stretch" 拉伸
annotationToolbar.getAnnotationManager().setOperationMode(["Delete", "Edit", "Rotate", "Stretch"]);
```

## 监听绘制完成事件

```javascript
// 每次批注图元绘制完成后触发回调
annotationToolbar.getAnnotationManager().itemCompleted(function(data) {
    const type = data.markupType; // 批注类型：Arrow/RectCloud/Cloud/Rect/Ellipse/Cross/Text
    console.log("绘制完成:", type);
});
```

## 捕捉模式（批注中启用）

```javascript
// 构造捕捉模式
const snapMode = new Glodon.Bimface.Viewer.SnapMode();
// 2D场景支持的捕捉对象：Endpoint(端点)、Midpoint(中点)、Intersection(交点)、Perpendicular(垂足)、Line(线)
snapMode.setSnap2DList([
    Glodon.Bimface.Viewer.SnapObject.Endpoint,
    Glodon.Bimface.Viewer.SnapObject.Midpoint,
    Glodon.Bimface.Viewer.SnapObject.Intersection,
    Glodon.Bimface.Viewer.SnapObject.Perpendicular
]);
// 应用捕捉模式
viewer2D.setSnapMode(snapMode);
// 开启捕捉
viewer2D.enableSnap(true);
// 关闭捕捉
viewer2D.enableSnap(false);
```
