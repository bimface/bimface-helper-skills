# 图纸测量

## 使用约束与说明
- 必须在图纸 `Loaded` 事件触发后才能初始化测量对象
- Layout测量时，仅可在同一个视口内或仅在视口外进行测量
- 视口内测量的尺寸按视口映射关系和缩放比例换算为Model空间尺寸

## 初始化测量对象

```javascript
// 设置测量的配置项
const measureConfig = new Glodon.Bimface.Plugins.Measure.MeasureConfig();
measureConfig.viewer = viewer2D;

// 创建测量对象
const measure = new Glodon.Bimface.Plugins.Measure.Measure(measureConfig);
```

## 开启距离测量

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

// 面积测量（2D图纸支持）
measure.setMeasureType(Glodon.Bimface.Plugins.Measure.MeasureTypeOption.Area);

// 角度测量
measure.setMeasureType(Glodon.Bimface.Plugins.Measure.MeasureTypeOption.Angle);
```

## 获取测量数据

```javascript
// 获取所有测量项
const measureItems = measure.getAllItems();
console.log(measureItems);
```

## 设置测量数据

```javascript
// 根据数据列表恢复测量数据
const items = [
    {
        "type": "Distance",
        "id": "515f6f48-a58f-4e60-a892-a42d47de702d",
        "points": [
            { "x": -9971.22, "y": -893.18 },
            { "x": 7824.18, "y": -893.18 }
        ],
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
measure.clear();
```

## 退出测量

```javascript
measure.switchOff();
```
