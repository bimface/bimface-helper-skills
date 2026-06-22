# SDK 初始化流程

## 使用约束与说明

- 严格参照本文档的接口调用方式，不自行变更方法名、参数名
- v4 版本去除了所有 `*Config` 类，构造参数直接传入对象
- **推荐使用 ESM import 方式引入 SDK**，兼容 script 标签方式
- `model` 对象必须在模型加载完成后才能获取（在 `ViewAdded` 事件回调或 `loadModel` Promise 回调中）
- 语言设置通过 `BimfaceSDKLoader.load()` 的 `language` 选项传入
- **全局变量名统一使用**：`viewer3D`、`app`、`model`、`camera`
- **相机通过 `viewer3D.getCamera()` 获取**，在 `ViewAdded` 事件后可用
- **视图元数据（viewMetaData）模式**是 v4 主要加载方式：`viewer3D.loadModel({viewMetaData, modelId: 'model'})`

---

## 1. 引入 SDK Loader 脚本（ESM 方式，推荐）

```javascript
// ESM 动态 import（主要方式，推荐）
const {BimfaceSDKLoader} = await import('https://static.bimface.com/api/BimfaceSDKLoader/BimfaceSDKLoader-v4.esm.js');
```

备选：script 标签方式（兼容旧版）：

```html
<script src="https://static.bimface.com/api/BimfaceSDKLoader/BimfaceSDKLoader-v4@latest-release.js"></script>
```

---

## 2. 加载 SDK 并获取视图元数据

`BimfaceSDKLoader.load()` 接收对象参数（无 Config 类），**返回 Promise，解析为 `viewMetaData` 对象**：

```javascript
// ESM 方式加载
const {BimfaceSDKLoader} = await import('https://static.bimface.com/api/BimfaceSDKLoader/BimfaceSDKLoader-v4.esm.js');

// 加载 SDK，获取 viewMetaData
const viewMetaData = await BimfaceSDKLoader.load({
    viewToken: "your-view-token",
    language: Glodon.Bimface.LanguageOption.en_GB  // 可选，默认中文
});
// viewMetaData 将用于后续 loadModel
```

> **重要**：`BimfaceSDKLoader.load()` 在 v4 中 **返回 viewMetaData 对象**，而非仅仅加载 SDK。`viewMetaData` 必须传入 `viewer3D.loadModel()`。

---

## 3. 创建 WebApplication3D 应用

无需 Config 类，直接传入 `domElement` 和工具配置：

```javascript
const app = new Glodon.Bimface.Application.WebApplication3D({
    domElement: document.getElementById("bimface-container"),
    tool: {
        toolbar: { visible: true },   // 显示主工具栏
        tree: { visible: true }        // 显示构件树
    }
});
```

> `tool` 配置（均为可选）：`toolbar.visible`、`tree.visible`

---

## 4. 创建 Viewer3D 查看器

使用 `app.createViewer()` 创建查看器，传入配置对象：

```javascript
const viewer3D = app.createViewer({
    userInteraction: {
        contextMenu: true,    // 启用右键菜单
        hover: false          // 禁用悬停高亮
    },
    performance: {
        memoryThreshold: 2048  // 可选，内存阈值（MB）
    },
    visualization: {
        ambientOcclusion: false,   // 环境光遮蔽
        csmShadow: false,           // 阴影
        wireframe: false,           // 线框模式
        envMap: false,              // 环境贴图
        exposure: 1.0,              // 曝光度
        backgroundColor: new Glodon.Bimface.Common.Graphics.Color(0.2, 0.2, 0.2, 1.0)
    }
    // visualization 中所有属性均为可选，按需配置
});
```

---

## 5. 加载模型

### 主要方式：通过 viewMetaData 加载（推荐）

```javascript
// 使用 BimfaceSDKLoader.load() 返回的 viewMetaData
const viewMetaData = await BimfaceSDKLoader.load({ viewToken: "your-view-token" });

// viewMetaData 直接传入 loadModel
const model = await viewer3D.loadModel({
    viewMetaData: viewMetaData,
    modelId: 'model'          // 默认模型 ID
});
```

### 多模型加载

```javascript
// 加载第二个模型（使用 viewToken 直接加载）
const model2 = await viewer3D.loadModel({
    viewToken: "second-view-token",
    modelId: "second-model"
});

// 用 viewMetaData 加载第二个模型也可以
const viewMetaData2 = await BimfaceSDKLoader.load({ viewToken: "third-view-token" });
const model3 = await viewer3D.loadModel({
    viewMetaData: viewMetaData2,
    modelId: "third-model"
});
```

> **注意**：`ViewAdded` 事件触发后，`viewer3D.getModel()` 才能正常获取到模型对象。如果页面有多个模型，需要通过 `viewer3D.getModel(modelId)` 指定目标模型。

---

## 6. 获取相机

相机通过 `viewer3D.getCamera()` 同步获取，**须在 `ViewAdded` 事件触发后**：

```javascript
const camera = viewer3D.getCamera();
```

---

## 7. Widget 初始化与获取

```javascript
// 初始化 Widget
app.initializeWidget("WidgetName");

// 获取 Widget 实例
const widget = app.getWidget("WidgetName");
```

### 主工具栏

```javascript
// 获取主工具栏
const mainToolbar = app.getToolbar("MainToolbar");

// 显示/隐藏工具栏
mainToolbar.show();
mainToolbar.hide();
```

---

## 完整示例

### ESM 方式（推荐）

```javascript
// 全局变量
let viewer3D, app, model, camera;

(async function() {
    // 1. ESM 导入 SDK Loader
    const {BimfaceSDKLoader} = await import('https://static.bimface.com/api/BimfaceSDKLoader/BimfaceSDKLoader-v4.esm.js');

    // 2. 加载 SDK，获取 viewMetaData
    const viewMetaData = await BimfaceSDKLoader.load({
        viewToken: "your-view-token",
        language: Glodon.Bimface.LanguageOption.en_GB
    });

    // 3. 创建 WebApplication3D
    app = new Glodon.Bimface.Application.WebApplication3D({
        domElement: document.getElementById("bimface-container"),
        tool: {
            toolbar: { visible: true },
            tree: { visible: true }
        }
    });

    // 4. 创建 Viewer3D
    viewer3D = app.createViewer({
        userInteraction: {
            contextMenu: true,
            hover: false
        },
        performance: {
            memoryThreshold: 2048
        },
        visualization: {
            ambientOcclusion: false,
            csmShadow: false,
            wireframe: false,
            envMap: false,
            exposure: 1.0,
            backgroundColor: new Glodon.Bimface.Common.Graphics.Color(0.2, 0.2, 0.2, 1.0)
        }
    });

    // 5. 监听 ViewAdded 事件，获取 model 和 camera
    viewer3D.addEventListener(
        Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
        function() {
            model = viewer3D.getModel();
            camera = viewer3D.getCamera();
        }
    );

    // 6. 加载模型（使用 viewMetaData）
    model = await viewer3D.loadModel({
        viewMetaData: viewMetaData,
        modelId: 'model'
    });

    console.log("模型加载完成");
})();
```

### Script 标签方式（兼容）

```javascript
var viewer3D, app, model, camera;

BimfaceSDKLoader.load({
    viewToken: "your-view-token",
    language: Glodon.Bimface.LanguageOption.en_GB
}).then(function(viewMetaData) {

    // 创建 WebApplication3D
    app = new Glodon.Bimface.Application.WebApplication3D({
        domElement: document.getElementById("bimface-container"),
        tool: {
            toolbar: { visible: true },
            tree: { visible: true }
        }
    });

    // 创建 Viewer3D
    viewer3D = app.createViewer({
        userInteraction: {
            contextMenu: true,
            hover: false
        },
        performance: {
            memoryThreshold: 2048
        },
        visualization: {
            ambientOcclusion: false,
            csmShadow: false,
            backgroundColor: new Glodon.Bimface.Common.Graphics.Color(0.2, 0.2, 0.2, 1.0)
        }
    });

    // 监听 ViewAdded 事件
    viewer3D.addEventListener(
        Glodon.Bimface.Viewer.Viewer3DEvent.ViewAdded,
        function() {
            model = viewer3D.getModel();
            camera = viewer3D.getCamera();
        }
    );

    // 加载模型
    viewer3D.loadModel({
        viewMetaData: viewMetaData,
        modelId: 'model'
    }).then(function(loadedModel) {
        model = loadedModel;
    });

}, function(error) {
    console.error("SDK 加载失败:", error);
});
```
