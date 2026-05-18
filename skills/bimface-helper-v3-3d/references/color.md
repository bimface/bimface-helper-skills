# 场景内的构件及图元颜色

## 构造颜色对象
需要通过BIMFACE提供的Color类进行颜色对象的定义，可以通过RGB或十六进制颜色值进行定义。
```JavaScript
const color_1 = new Glodon.Web.Graphics.Color(red, green, blue, alpha)
const color_2 = new Glodon.Web.Graphics.Color("#RRGGBB", alpha)
```
其中，alpha的取值范围为0-1，0表示完全透明，1表示完全不透明，默认值为1；red、green、blue每个通道的取值范围为0-255

## 修改构件颜色

1. 根据构件ID列表设置颜色
```JavaScript
// 给指定构件ID列表设置颜色
// componentIds 是一个数组，包含要设置颜色的构件ID
const componentIds = ["123456", "789012"];
model.overrideComponentsColorById(componentIds, new Glodon.Web.Graphics.Color("#EE799F", 0.8));
viewer.render();
```

2. 根据筛选条件设置颜色
```JavaScript
// 给符合指定筛选条件的构件设置颜色
// filter 是一个筛选条件对象，包含要筛选的构件类型、属性等，参考[筛选条件](filter.md)
const filter = {
    categoryId: -2001340,
    levelName: "F1"
}
model.overrideComponentsColorByObjectData(filter, new Glodon.Web.Graphics.Color("#EE799F", 0.8));
viewer.render();
```

## 恢复构件原始颜色
```javascript
// 恢复指定构件ID列表的原始颜色
// componentIds 是一个数组，包含要恢复颜色的构件ID
const componentIds = ["123456", "789012"];
model.restoreComponentsColorById(componentIds);
viewer.render();
```

## 恢复全部构件的原始颜色
```JavaScript
// 恢复全部构件的原始颜色
model.clearOverrideColorComponents();
```