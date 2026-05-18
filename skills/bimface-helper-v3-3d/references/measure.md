# 模型测量

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能初始化测量对象
- 必须先 `setMeasureType` 设置测量类型，再 `switchOn` 开启测量

## 初始化测量对象

```javascript
// 设置测量的配置项
const measureConfig = new Glodon.Bimface.Plugins.Measure.MeasureConfig();
measureConfig.viewer = viewer3D;

// 创建测量对象
const measure = new Glodon.Bimface.Plugins.Measure.Measure(measureConfig);
```

## 测量长度（距离测量）

```javascript
// 设置测量类型为距离测量
measure.setMeasureType(Glodon.Bimface.Plugins.Measure.MeasureTypeOption.Distance);

// 开启测量
measure.switchOn();
```

## 测量类型说明

```javascript
// 距离测量
measure.setMeasureType(Glodon.Bimface.Plugins.Measure.MeasureTypeOption.Distance);

// 最小距离测量
measure.setMeasureType(Glodon.Bimface.Plugins.Measure.MeasureTypeOption.MinimumDistance);

// 角度测量
measure.setMeasureType(Glodon.Bimface.Plugins.Measure.MeasureTypeOption.Angle);

// 标高测量
measure.setMeasureType(Glodon.Bimface.Plugins.Measure.MeasureTypeOption.Elevation);
```

## 获取测量数据

```javascript
// 获取所有测量项
const measureItems = measure.getAllItems();
console.log(measureItems);
```

## 设置测量数据

```javascript
// 根据数据列表设置测量数据
const items = [
    {
        "type": "Distance",
        "id": "515f6f48-a58f-4e60-a892-a42d47de702d",
        "points": [
            { "x": -9971.216327402833, "y": -893.1815985846936, "z": 13370.067730055925 },
            { "x": 7824.184997613858, "y": -893.1815985846936, "z": 13370.067730055925 }
        ],
        "distanceX": 17795,
        "distanceY": 0,
        "distanceZ": 0,
        "distance": 17795,
        "precision": 0,
        "scale": 1,
        "lengthUnits": "None"
    }
];
measure.setItems(items);
```

## 隐藏/显示测量数据

```javascript
// 隐藏所有测量数据
measure.hideAllItems();

// 显示所有测量数据
measure.showAllItems();
```

## 清除测量数据

```javascript
// 清除所有测量数据
measure.clear();
```

## 退出测量

```javascript
// 退出测量模式
measure.switchOff();
```
