# 模型状态

## 使用约束与说明
- 保存/恢复状态必须在`ViewAdded`事件触发后、场景加载完成时才能调用，否则`getCurrentState()`返回的是空白状态
- `setState` 恢复状态后需调用 `viewer3D.render()` 刷新场景
- 保存的状态包含：相机位置、构件隔离、隐藏、半透明等信息

## 保存当前状态

```javascript
const savedState = viewer3D.getCurrentState();
```

## 恢复已保存的状态

```javascript
viewer3D.setState(savedState);
viewer3D.render();
```
