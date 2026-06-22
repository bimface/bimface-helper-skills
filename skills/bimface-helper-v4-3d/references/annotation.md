# 模型批注（Annotation）

## 使用约束与说明

- 批注功能通过 Widget 模式初始化和使用：先 `app.initializeWidget("AnnotationToolbar")`，再 `app.getWidget("AnnotationToolbar")` 获取 toolbar 实例
- 命名空间为 `Plugin`（单数），非 `Plugins`
- AnnotationManager 由 `annotationToolbar.getAnnotationManager()` 同步获取
- `getStatus()` 返回 Promise，需通过 `.then()` 或 `await` 处理
- 进入批注模式前建议隐藏主工具栏 `app.getToolbar("MainToolbar").hide()`，退出时恢复 `.show()`
- 所有操作必须在模型加载完成后执行

---

## 初始化 AnnotationToolbar

```javascript
// 初始化 Widget
app.initializeWidget("AnnotationToolbar");
// 获取 toolbar 实例
const annotationToolbar = app.getWidget("AnnotationToolbar");
```

---

## Toolbar 显隐

```javascript
// 显示批注工具栏
annotationToolbar.show();

// 隐藏批注工具栏
annotationToolbar.hide();
```

---

## Toolbar 事件

### onSave —— 保存批注回调

```javascript
annotationToolbar.onSave((data) => {
  console.log("批注已保存:", data);
  // data 包含批注数据，可持久化到服务端
});
```

### onCancel —— 取消批注回调

```javascript
annotationToolbar.onCancel(() => {
  console.log("批注已取消");
});
```

---

## 获取 AnnotationManager

```javascript
const annotationManager = annotationToolbar.getAnnotationManager();
```

---

## AnnotationManager 方法

### getStatus —— 获取批注状态

返回 Promise，resolve 后得到批注状态对象：

```javascript
annotationManager.getStatus().then((status) => {
  console.log("批注状态:", status);
  // 可将 status 保存，用于后续恢复
  savedAnnotationStatus = status;
});
```

### setStatus —— 恢复批注状态

```javascript
// 将之前保存的状态恢复
annotationManager.setStatus(savedAnnotationStatus);
```

### createSnapshot —— 导出批注快照

```javascript
annotationManager.createSnapshot((imgData) => {
  // imgData 为导出的图像数据
  console.log("快照已生成:", imgData);
  // 可用于下载或展示
});
```

### enablePick —— 切换拾取/选择模式

```javascript
// 启用拾取模式
annotationManager.enablePick(true);

// 禁用拾取模式
annotationManager.enablePick(false);
```

### setOperationMode —— 设置允许的操作模式

```javascript
// 允许所有操作（默认）
annotationManager.setOperationMode(["Delete", "Edit", "Move", "Rotate", "Stretch"]);

// 仅允许移动和删除
annotationManager.setOperationMode(["Move", "Delete"]);

// 禁止所有操作（只读浏览）
annotationManager.setOperationMode([]);
```

| 模式 | 说明 |
|------|------|
| `Delete` | 删除批注 |
| `Edit` | 编辑批注文本 |
| `Move` | 移动批注位置 |
| `Rotate` | 旋转批注 |
| `Stretch` | 拉伸批注 |

### exit —— 退出批注模式

```javascript
annotationManager.exit();
```

---

## 完整示例

```javascript
const viewer3D = new Glodon.Bimface.Viewer.Viewer3D(viewer3DConfig);
const app = viewer3D.getApp();
viewer3D.addModel(viewToken);
const model = viewer3D.getModel();

viewer3D.addEventListener(Glodon.Bimface.Viewer.Viewer3DEvent.ModelLoaded, () => {
  // 初始化批注工具栏
  app.initializeWidget("AnnotationToolbar");
  const annotationToolbar = app.getWidget("AnnotationToolbar");

  // 注册保存事件
  annotationToolbar.onSave((data) => {
    console.log("批注已保存:", data);
  });

  // 注册取消事件
  annotationToolbar.onCancel(() => {
    console.log("批注已取消");
  });

  // 获取 AnnotationManager
  const annotationManager = annotationToolbar.getAnnotationManager();

  // 设置操作模式
  annotationManager.setOperationMode(["Delete", "Edit", "Move", "Rotate", "Stretch"]);

  // 进入批注模式
  document.getElementById("btnStartAnnotation").addEventListener("click", () => {
    // 隐藏主工具栏
    app.getToolbar("MainToolbar").hide();
    // 显示批注工具栏
    annotationToolbar.show();
  });

  // 退出批注模式
  document.getElementById("btnExitAnnotation").addEventListener("click", () => {
    annotationManager.exit();
    annotationToolbar.hide();
    // 恢复主工具栏
    app.getToolbar("MainToolbar").show();
  });

  // 保存批注状态
  document.getElementById("btnSaveStatus").addEventListener("click", () => {
    annotationManager.getStatus().then((status) => {
      console.log("当前批注状态:", status);
      // 可存储到服务端
      localStorage.setItem("annotationStatus", JSON.stringify(status));
    });
  });

  // 恢复批注状态
  document.getElementById("btnRestoreStatus").addEventListener("click", () => {
    const savedStatus = JSON.parse(localStorage.getItem("annotationStatus"));
    if (savedStatus) {
      annotationManager.setStatus(savedStatus);
    }
  });

  // 导出批注快照
  document.getElementById("btnSnapshot").addEventListener("click", () => {
    annotationManager.createSnapshot((imgData) => {
      console.log("批注快照已生成");
      // 例如：创建下载链接
      const link = document.createElement("a");
      link.href = imgData;
      link.download = "annotation-snapshot.png";
      link.click();
    });
  });
});
```

---

## 注意事项

- 进入批注模式前务必隐藏主工具栏，避免 UI 冲突
- `getStatus()` 是异步方法，不能用同步方式获取返回值
- `setStatus(status)` 用于场景恢复，配合 `getStatus()` 可实现批注状态的持久化
- 操作模式数组为空时，批注进入只读浏览状态
