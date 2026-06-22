# 模型编辑器（ModelEditor）

> ⚠️ **注意**：本文档基于 v3 API 文档 `bimface-helper-v3-3d` 和 v4 通用迁移模式（命名空间 `Plugins→Plugin`、Config 类去除、Widget 机制等）编写，具体 API 请以 `参考/接口文档/ModelEditor-v4.pdf` 为准。

## 使用约束与说明

- **Widget 模式**：v4 中模型编辑器作为 Widget 初始化，使用 `app.initializeWidget("ModelEditor")` 和 `app.getWidget("ModelEditor")` 获取实例
- **Promise 化**：`getStatus()` 等方法返回 Promise
- 模型编辑器可对模型进行平移、旋转、缩放等编辑操作
- 对内部模型构件编辑前，需先将构件转为外部构件（参考 [外部构件](externalObject.md) 中的 `convert()` 方法）
- 编辑变更需通过 BIMFACE 后端 API 上传保存，前端仅负责编辑和获取状态

---

## 创建模型编辑器

```javascript
// 初始化模型编辑器 Widget
app.initializeWidget("ModelEditor");

// 获取模型编辑器实例
const modelEditor = app.getWidget("ModelEditor");

// 打开编辑器
modelEditor.show();
```

## 获取编辑器状态

```javascript
// getStatus 返回 Promise
modelEditor.getStatus().then((status) => {
  console.log("当前编辑器状态:", status);
  // status 结构示例：
  // {
  //   isVisible: true,        // 编辑器是否可见
  //   translate: { x, y, z }, // 平移量
  //   rotate: { x, y, z },    // 旋转量
  //   scale: { x, y, z }      // 缩放量
  // }
});
```

## 设置编辑器状态

```javascript
// 设置编辑器状态
modelEditor.setStatus({
  translate: { x: 100, y: 200, z: 0 },
  rotate: { x: 0, y: 0, z: 0 },
  scale: { x: 1, y: 1, z: 1 }
});
```

## 显示/隐藏编辑器

```javascript
// 显示编辑器工具条
modelEditor.show();

// 隐藏编辑器工具条
modelEditor.hide();
```

## 设置编辑器目标模型

```javascript
// 指定需要编辑的模型
modelEditor.setTarget({
  modelId: "10000776931924"  // 目标模型ID
});

// 指定需要编辑的构件（先转为外部构件）
// 详见 externalObject.md 中的 convert() 方法
modelEditor.setTarget({
  objectId: extObjMng.getObjectIdByName("targetComponent")
});
```

---

## 工具条模式（Toolbar）

除 Widget 模式外，v4 也可能支持工具条（Toolbar）方式直接创建编辑控件。

### 模型编辑器工具条

```javascript
// v4 中不再使用 ModelEditorToolbarConfig，直接传入配置对象
const editor = new Glodon.Bimface.Plugin.ModelEditor.ModelEditorToolbar({
  ids: ["10000776931924"],  // 需要编辑的模型ID列表
  viewer: viewer3D          // Viewer3D 实例
});

// 设置工具条按钮可见性
editor.setButtonVisibility({
  translate: true,   // 开启平移按钮
  rotate: true,      // 开启旋转按钮
  scale: true        // 开启缩放按钮
});

// 设置平移控制器
editor.setTranslationController({
  X: true,
  Y: true,
  Z: true
});

// 显示工具条
editor.show();
```

### 外部构件编辑器工具条

```javascript
// 创建外部构件管理器
const extObjMng = new Glodon.Bimface.Plugin.ExternalObject.ExternalObjectManager({
  viewer: viewer3D
});

// 加载外部构件
extObjMng.loadObject({
  name: "vehicle",
  url: { objectUrl: "https://example.com/vehicle.fbx" }
}).then(() => {
  const extObjId = extObjMng.getObjectIdByName("vehicle");

  // 创建外部构件编辑器工具条
  const toolbar = new Glodon.Bimface.Plugin.ModelEditor.ExternalObjectEditorToolbar({
    viewer: viewer3D,
    id: extObjId
  });

  toolbar.show();
});
```

---

## 完整示例（Widget 模式）

```javascript
// 在 ViewAdded 事件后初始化编辑功能
viewer3D.addEventListener(
  Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
  () => {
    // 1. 初始化模型编辑器 Widget
    app.initializeWidget("ModelEditor");
    const modelEditor = app.getWidget("ModelEditor");

    // 2. 设置编辑目标
    modelEditor.setTarget({ modelId: "10000776931924" });

    // 3. 显示编辑器
    modelEditor.show();

    // 4. 异步获取当前状态
    modelEditor.getStatus().then((status) => {
      console.log("当前变换:", status);
    });
  }
);

// 某个事件触发时保存编辑结果
async function saveEditResult() {
  const status = await modelEditor.getStatus();
  // 通过 BIMFACE 后端 API 上传编辑结果
  // POST /api/upload-edit 等...
}
```

---

## v3 → v4 对照

| 项目 | v3（旧版） | v4（新版） |
|------|-----------|-----------|
| 初始化方式 | `new ModelEditorToolbar(config)` | Widget: `app.initializeWidget("ModelEditor")` |
| 获取实例 | 直接引用 | `app.getWidget("ModelEditor")` |
| 获取状态 | `getState()` 同步 | `getStatus()` 返回 Promise |
| 设置状态 | `setState(state)` | `setStatus(status)` |
| Config 类 | `ModelEditorToolbarConfig` / `ExternalObjectEditorToolbarConfig` | 不再需要，直接传对象 |
| 命名空间 | `Plugins.ModelEditor` / `Plugins.ExternalObject` | `Plugin.ModelEditor` / `Plugin.ExternalObject` |
| 目标设置 | 通过 Config.id 或 Config.ids | `setTarget({modelId/objectId})` |

> **提示**：关于外部构件编辑（移动、旋转、缩放内部构件），请先参考 [外部构件](externalObject.md) 文档中的 `convert()` 方法将内部构件转为外部构件后操作。
