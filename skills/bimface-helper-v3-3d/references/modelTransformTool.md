# 合模工具

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能创建合模工具

## 开启合模工具

```javascript
// 构造合模工具配置项
const modelTransformToolConfig = new Glodon.Bimface.Plugins.ModelTransform.ModelTransformToolConfig();
// 设置Viewer对象
modelTransformToolConfig.viewer = viewer3D;
// 构造合模工具
const modelTransformTool = new Glodon.Bimface.Plugins.ModelTransform.ModelTransformTool(modelTransformToolConfig);
```

## 获取模型坐标数据

```javascript
// 获取场景内所有模型当前坐标数据
const data = modelTransformTool.getModelTransformData();
```
