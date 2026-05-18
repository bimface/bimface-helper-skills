# 尺寸标注

## 使用约束与说明
- 必须在`ViewAdded`事件触发后才能操作尺寸标注
- `getDimensions()` 是异步回调，结果必须在回调函数中处理
- 尺寸标注与模型测量（Measure）是两套独立的 API 体系；Measure 用于交互式测量，Dimension 用于显示模型预制尺寸或程序创建持久尺寸

## 获取模型预制尺寸（异步）

```javascript
// 从模型中读取已有的尺寸标注数据
viewer3D.getModel().getDimensions('', function (data) {
    // data.data 为尺寸数据数组，可用于初始化 DimensionContainer
    const dimensionData = data.data;
    console.log(dimensionData);
});
```

## 创建尺寸标注容器

```javascript
const dimensionContainerConfig = new Glodon.Bimface.Plugins.Dimension.DimensionContainerConfig();
dimensionContainerConfig.viewer = viewer3D;
dimensionContainerConfig.contents = dimensionData;  // 从模型获取或自行构造的尺寸数据

const dimensionContainer = new Glodon.Bimface.Plugins.Dimension.DimensionContainer(dimensionContainerConfig);
```

## 显示控制

```javascript
// 显示所有尺寸
dimensionContainer.showAllItems();

// 隐藏所有尺寸
dimensionContainer.hideAllItems();
```

## 添加线性尺寸

### 构造线性尺寸内容

```javascript
const item = {
    dimension: {
        distance: 17795,                     // 标注距离值
        type: "Distance",                    // 尺寸类型
        styleType: "Linear",                 // 标注样式：线性
        unit: "Millimeter",                  // 单位
        points: [                            // 标注的两个端点
            { x: -9971.21, y: -893.18, z: 13370.06 },
            { x: 7824.18, y: -893.18, z: 13370.06 }
        ]
    },
};
```

### 添加带自定义文字的尺寸

```javascript
const item = {
    dimension: {
        distance: 6104,
        type: "Distance",
        styleType: "Linear",
        unit: "Millimeter",
        text: "宽度",                         // 自定义文字，显示在标注线上方
        points: [
            { x: -9471.21, y: -4267.28, z: 5141.31 },
            { x: -9471.21, y: 1836.79, z: 5141.34 }
        ]
    },
};
```

### 创建并添加到容器

```javascript
const linearConfig = new Glodon.Bimface.Plugins.Dimension.LinearConfig();
linearConfig.color = new Glodon.Web.Graphics.Color("#EE799F", 0.3);
linearConfig.viewer = viewer3D;

const linear = new Glodon.Bimface.Plugins.Dimension.Linear(linearConfig);
linear.setContent(item);

// 添加到容器（单个添加）
dimensionContainer.addItem(linear);

// 批量添加
dimensionContainer.addItems([linear2, linear3]);
```
