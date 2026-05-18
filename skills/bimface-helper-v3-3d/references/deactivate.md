# 冻结构件

## 功能说明
冻结后，构件**无法被选中**、**无法修改颜色与材质**、**无法控制可见性（显示/隐藏）**、**无法设置隔离效果**，常用于临时固定某些构件。

## 使用约束与说明
- 必须在`ViewAdded`事件触发后、`model3D`可用时才能调用
- 冻结/取消冻结后需调用 `viewer3D.render()` 刷新场景

## 根据构件ID冻结

```javascript
model3D.deactivateComponentsById(['267327', '268067']);
viewer3D.render();
```

## 根据筛选条件冻结

> `deactivateComponentsByObjectData` 的参数格式需参照 [构件筛选](filter.md) 中的筛选条件定义

```javascript
const conditions = [
  { levelName: "F02" },
  { categoryId: -2001340, family: "Basic Wall" }
];
model3D.deactivateComponentsByObjectData(conditions);
viewer3D.render();
```

## 取消全部冻结

```javascript
model3D.activateAllComponents();
viewer3D.render();
```
