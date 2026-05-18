# 隔离构件

## 使用约束与说明
- 必须在`ViewAdded`事件触发后、`model3D`可用时才能调用
- 隔离/取消隔离后需调用 `viewer3D.render()` 刷新场景
- 非隔离构件默认变为半透明，隔离期间非隔离构件不可被选中

## 根据构件ID隔离

```javascript
const makeOthersTranslucent = Glodon.Bimface.Viewer.IsolateOption.MakeOthersTranslucent;
model3D.isolateComponentsById(['390274', '393020', '393021'], makeOthersTranslucent);
viewer3D.render();
```

> 构件ID格式为合成ID：`"文件ID.原始构件ID"`（例如 `"10000909527733.393020"`），适用于多模型场景。

## 根据筛选条件隔离

> `isolateComponentsByObjectData` 的条件参数格式需参照 [构件筛选](filter.md) 中的筛选条件定义。

```javascript
const conditions = [
  { levelName: "F1" },
  { levelName: "F2" }
];
model3D.isolateComponentsByObjectData(conditions, makeOthersTranslucent);
viewer3D.render();
```

## 取消隔离

```javascript
model3D.clearIsolation();
viewer3D.render();
```
