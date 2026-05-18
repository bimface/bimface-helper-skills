# 模型属性

## 使用约束与说明
- 必须在`ViewAdded`事件触发后、`model3D`可用时才能调用
- `getComponentProperty`、`getFloors`、`getModelTree` 都是异步回调，结果必须在回调函数中处理，不能同步赋值
- 多模型场景下必须用 `viewer3D.getModel(modelId)` 指定目标模型

## 获取构件属性

```javascript
// 根据构件ID获取构件属性
viewer3D.getModel().getComponentProperty("307240", function (objectdata) {
    console.log(objectdata);
});
```

### 返回值结构

callback返回的`objectdata`包含以下字段：

```javascript
{
    code: "success",           // 状态码，成功为"success"
    message: null,             // 错误信息，成功时为null
    data: {
        boundingBox: {         // 构件包围盒
            max: { x: 7.3288, y: 4.9577, z: 13.37 },   // 最大坐标点
            min: { x: -9.4712, y: -6.7443, z: 9.53 }   // 最小坐标点
        },
        elementId: "267327",   // 构件元素ID
        familyGuid: "-1",      // 族GUID
        guid: "dba8ee4c-c31c-4754-9bd7-30f3b2d7e496-0004143f",  // 构件GUID
        name: "基本屋顶",       // 构件名称
        properties: [           // 属性分组数组
            {
                group: "基本属性",        // 属性组名称
                items: [                 // 属性项数组
                    {
                        key: "参数名称",  // 属性键名
                        unit: "mm",       // 单位（可为空字符串）
                        value: "值",      // 属性值
                        valueType: 1      // 值类型：1-字符串，2-数值，3-GUID，4-枚举
                    }
                ]
            }
            // 更多属性组...
        ]
    }
}
```

### valueType 类型说明

| 值 | 类型 | 说明 |
|-----|------|------|
| 1 | 字符串 | 普通文本值，如"默认"、"否" |
| 2 | 数值 | 带单位的数值，如"75.96" |
| 3 | GUID | 全局唯一标识符，如构件ID |
| 4 | 枚举 | 枚举类型值，如"新构造"、"基本屋顶" |

## 获取楼层信息

```javascript
// 获取模型的所有楼层信息
viewer3D.getModel().getFloors(function (objectdata) {
    console.log(objectdata);
});
```

## 获取目录树信息

```javascript
// 获取模型的目录树结构
viewer3D.getModel().getModelTree(function (objectdata) {
    console.log(objectdata);
});
```
