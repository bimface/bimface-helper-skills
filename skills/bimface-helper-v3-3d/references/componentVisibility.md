# 构件可见性

## 使用约束与说明
- 必须在`ViewAdded`事件触发后、`model3D`可用时才能调用
- 显示/隐藏/半透明操作后需调用 `viewer3D.render()` 刷新场景
- 半透明状态下，构件不可被选中
- `*ByObjectData` 系列方法的筛选条件格式需参照 [构件筛选](filter.md)

## 显示构件

```Javascript
// 显示指定构件ID列表
const objectIds = ["123456", "789012"];
model3D.showComponentsById(objectIds);
viewer3D.render();

// 显示符合筛选条件的构件
const filter = [{
    category: "Wall", levelName: "F2" // 筛选F2层的墙
}];
model3D.showComponentsByObjectData(filter);
viewer3D.render();

// 显示所有构件
model3D.showAllComponents();
viewer3D.render();
```

## 隐藏构件

```Javascript
// 隐藏指定构件ID列表
const objectIds = ["123456", "789012"];
model3D.hideComponentsById(objectIds);
viewer3D.render();

// 隐藏符合筛选条件的构件
const filter = [{
    category: "Wall", levelName: "F2" // 筛选F2层的墙
}];
model3D.hideComponentsByObjectData(filter);
viewer3D.render();

// 隐藏所有构件
model3D.hideAllComponents();
viewer3D.render();
```

## 半透明构件

```Javascript
// 半透明指定构件ID列表
const objectIds = ["123456", "789012"];
model3D.transparentComponentsById(objectIds);
viewer3D.render();

// 半透明符合筛选条件的构件
const filter = [{
    category: "Wall", levelName: "F2" // 筛选F2层的墙
}];
model3D.transparentComponentsByObjectData(filter);
viewer3D.render();

// 半透明所有构件
model3D.transparentAllComponents();
viewer3D.render();
```

## 取消半透明状态

```Javascript
// 取消指定构件ID列表的半透明状态
const objectIds = ["123456", "789012"];
model3D.opaqueComponentsById(objectIds);
viewer3D.render();

// 取消符合筛选条件的构件的半透明状态
const filter = [{
    category: "Wall", levelName: "F2" // 筛选F2层的墙
}];
model3D.opaqueComponentsByObjectData(filter);
viewer3D.render();

// 取消所有构件的半透明状态
model3D.opaqueAllComponents();
viewer3D.render();
```
