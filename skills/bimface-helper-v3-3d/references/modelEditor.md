# 模型编辑器

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能创建模型编辑器工具条
- 模型编辑器可对模型进行平移、旋转、缩放等编辑操作

## 创建模型编辑器工具条

```javascript
// 设置模型编辑器工具条的配置项
const modelToolbarConfig = new Glodon.Bimface.Plugins.ModelEditor.ModelEditorToolbarConfig();
// 需要编辑的模型ID
modelToolbarConfig.ids = ['10000776931924'];
// 视图对象
modelToolbarConfig.viewer = viewer3D;
// 创建模型编辑器工具条
const editor = new Glodon.Bimface.Plugins.ModelEditor.ModelEditorToolbar(modelToolbarConfig);
```

## 设置编辑器工具条按钮可见性

```javascript
// 设置按钮可见性
editor.setButtonVisibility({  // 仅控制按钮的可见性，不涉及编辑状态的管理
    translate: true,  // 开启平移
    rotate: true,      // 开启旋转
    scale: true        // 开启缩放
});
```

## 设置平移编辑

```javascript
// 开启沿X、Y、Z轴的平移
editor.setTranslationController({
    X: true,
    Y: true,
    Z: true
});
```

## 显示编辑工具条

```javascript
editor.show();
```

## 创建外部构件编辑器工具条

```javascript
// 创建外部构件管理器
const extObjMng = new Glodon.Bimface.Plugins.ExternalObject.ExternalObjectManager(viewer3D);

// 加载外部构件
const objUrl = "https://example.com/model.3DS";
extObjMng.loadObject({name: 'modelName', url: {objectUrl: objUrl}}, function() {
    // 加载完成后的回调
});

// 获取构件ID
const objectId = extObjMng.getObjectIdByName("modelName");

// 平移构件
extObjMng.translate(objectId, {
    x: 14.920,
    y: -35.100,
    z: -0.150
});

// 缩放构件
extObjMng.scale(objectId, {
    x: 0.001,
    y: 0.001,
    z: 0.001
});

// 设置外部构件编辑器工具条的配置项
const toolbarConfig = new Glodon.Bimface.Plugins.ExternalObject.ExternalObjectEditorToolbarConfig();
toolbarConfig.viewer = viewer3D;
toolbarConfig.id = extObjMng.getObjectIdByName("vehicle");
// 创建外部构件编辑器工具条
const editor = new Glodon.Bimface.Plugins.ExternalObject.ExternalObjectEditorToolbar(toolbarConfig);
// 显示编辑工具条
editor.show();
```
