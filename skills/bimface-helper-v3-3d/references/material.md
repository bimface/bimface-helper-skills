# 材质贴图

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能操作材质
- Canvas材质需在`WebApplication3DConfig`中启用 `enableReplaceMaterial: true`
- 修改材质后需调用 `viewer3D.render()`

## 构件贴图

```javascript
const materialConfig = new Glodon.Bimface.Plugins.Material.MaterialConfig();
materialConfig.viewer = viewer3D;
materialConfig.src = 'https://static.bimface.com/attach/xxx.jpg';
materialConfig.scale = [0.01, 0.01];

const material = new Glodon.Bimface.Plugins.Material.Material(materialConfig);

// 覆盖指定构件材质
material.overrideComponentsMaterialById(['307240', '308366'], 'fileId');

// 旋转贴图
material.setRotation(90);
viewer3D.render();
```

### 清除贴图

```javascript
material.clearOverrideComponentsMaterial('fileId');
viewer3D.render();
```

## Canvas材质

通过动态Canvas绘制内容作为构件贴图。

```javascript
const canvas = document.createElement('canvas');
canvas.width = 2500;
canvas.height = 800;
const ctx = canvas.getContext('2d');
ctx.fillStyle = '#32D3A6';
ctx.fillRect(0, 0, 2500, 800);
ctx.font = '200px Arial';
ctx.fillStyle = '#FFFFFF';
ctx.fillText('文本内容', 1250, 400);

const materialConfig = new Glodon.Bimface.Plugins.Material.MaterialConfig();
materialConfig.viewer = viewer3D;
materialConfig.canvas = canvas;

const material = new Glodon.Bimface.Plugins.Material.Material(materialConfig);
material.overrideComponentsMaterialById(['构件ID']);
viewer3D.render();
```
