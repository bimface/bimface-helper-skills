# 筛选条件

## 使用约束与说明

- 当其他接口描述中（如构件着色、隔离、隐藏等）提到根据构件的objectData属性进行筛选时，必须严格按照本文档定义的JSON格式构造filter参数
- 获取构件属性的方法getObjectDataById包含异步回调函数，绝不能写成同步赋值

## 哪些字段可以用于筛选

### 1. 常见筛选字段

不同类型的模型支持的筛选字段有所不同：

#### RVT、RFA等模型
- **categoryId**：构件类别ID
- **family**：构件族
- **familyType**：构件类型
- **levelName**：楼层名称
- **specialty**：专业

#### SKP、NWD等模型
- 字段不固定，从L0开始依次往下，如L0、L1、L2等

### 2. 如何获取可筛选字段

可以通过以下方式获取构件的可筛选字段：

```javascript
// 通过构件ID获取objectData信息，查看可筛选字段
const objectId = "123456";
const objectData = model3D.getObjectDataById(objectId);
console.log("可筛选字段:", Object.keys(objectData));
console.log("字段值:", objectData);
```

## 如何表达筛选条件

筛选条件为一个JSON数组，每个数组元素即一个筛选条件，不同元素之间为并集关系，单个元素内为交集关系。即：
- 数组的元素之间是OR关系。
- 同一个对象内部的多个键值对之间是AND关系。

```javascript
// 示例：筛选 categoryId = -2001340 且 levelName = "F01" 的构件，或者 levelName = "F02" 的构件
const filter = [
  {
    categoryId: -2001340,
    levelName: "F01"
  },
  {
    levelName: "F02"
  }
];
```