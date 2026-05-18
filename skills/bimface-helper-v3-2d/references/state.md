# 状态管理

## 使用约束与说明
- 必须在图纸 `Loaded` 事件触发后才能获取或设置状态
- 状态数据用于保存当前浏览状态，方便后续恢复

## 获取当前图纸状态

```javascript
// 获取当前图纸的浏览状态（视图位置、缩放比例等）
const currentState = viewer2D.getState();
// 可将 currentState 保存到业务系统（如后端接口、localStorage等）
```

## 恢复图纸状态

```javascript
// 恢复到之前保存的浏览状态
viewer2D.setState(previousState);
viewer2D.render();
```

## 完整保存与恢复示例

```javascript
// 保存状态
const savedState = viewer2D.getState();
localStorage.setItem('drawingState', JSON.stringify(savedState));

// 恢复状态
function restoreState() {
    const state = JSON.parse(localStorage.getItem('drawingState'));
    if (state) {
        viewer2D.setState(state);
        viewer2D.render();
    }
}
```
