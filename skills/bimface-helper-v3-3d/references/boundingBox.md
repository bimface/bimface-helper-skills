# 包围盒

## 一、包围盒的数据结构

包围盒（Bounding Box）是一个用于包围三维模型或构件的立方体结构，用于表示对象的空间范围。在BIMFACE中，包围盒通常以以下形式表示：

```JSON
{
    "min": {  // 最小点坐标
        "x": -9496.219279454494,
        "y": -4299.299971053061,
        "z": 999.9607157957682
    },
    "max": {  // 最大点坐标
        "x": 8443.779875881222,
        "y": 6382.751044860747,
        "z": 9475.048556778615
    }
}
```

## 二、包围盒的获取方式

### 1. 根据文件获取包围盒

```JavaScript
// 通过模型对象获取整个模型的包围盒
model.getBoundingBox(fileId, callback);  // fileId: 单文件ID或集成模型的子文件ID
```
callback返回的对象内容如下：
```JSON
{
  "currentBoundingBox": {  // 模型当前位置的包围盒，如果对模型进行了平移、旋转、缩放等操作，这个包围盒会改变
    "min": {
      "x": -9496.219279454494,
      "y": -4299.299971053061,
      "z": 999.9607157957682
    },
    "max": {
      "x": 8443.779875881222,
      "y": 6382.751044860747,
      "z": 9475.048556778615
    }
  },
  "originalBoundingBox": {  // 模型原始位置的包围盒
    "min": {
      "x": -9496.219279454494,
      "y": -4299.299971053061,
      "z": 999.9607157957682
    },
    "max": {
      "x": 8443.779875881222,
      "y": 6382.751044860747,
      "z": 9475.048556778615
    }
  }
}
```

### 2. 根据构件ID获取包围盒

该方法**同步返回**包围盒对象，无需回调函数。

```JavaScript
var bbox = model.getBoundingBoxById("123456");
```

**返回值结构**：
```JSON
{
    "min": {
        "x": -9496.219279454494,
        "y": -4299.299971053061,
        "z": 999.9607157957682
    },
    "max": {
        "x": 8443.779875881222,
        "y": 6382.751044860747,
        "z": 9475.048556778615
    }
}
```

> ⚠️ `getBoundingBoxById` 是 **同步**方法，与 `getComponentProperty`（异步回调）不同。如果构件不存在，返回的 `bbox` 可能为 `null` 或缺少 `min`/`max` 属性，调用前务必做防御性判断：
> ```javascript
> var bbox = model.getBoundingBoxById(componentId);
> if (bbox && bbox.min && bbox.max) {
>     var center = {
>         x: (bbox.min.x + bbox.max.x) / 2,
>         y: (bbox.min.y + bbox.max.y) / 2,
>         z: (bbox.min.z + bbox.max.z) / 2
>     };
>     // 使用 center 点进行标签定位等操作
> }
> ```

### 3. 整合多个包围盒为一个大的包围盒

当需要将多个对象的包围盒合并为一个整体的包围盒时，可以通过以下方法实现：


```JavaScript
// 假设有多个包围盒需要合并
const boundingBoxes = [
  { min: { x: 0, y: 0, z: 0 }, max: { x: 10, y: 10, z: 10 } },
  { min: { x: 5, y: 5, z: 5 }, max: { x: 15, y: 15, z: 15 } },
  { min: { x: 20, y: 20, z: 20 }, max: { x: 30, y: 30, z: 30 } }
];

// 计算合并后的包围盒
function mergeBoundingBoxes(boxes) {
  if (boxes.length === 0) return null;
  
  let mergedMin = { ...boxes[0].min };
  let mergedMax = { ...boxes[0].max };
  
  for (let i = 1; i < boxes.length; i++) {
    const box = boxes[i];
    // 更新最小点
    mergedMin.x = Math.min(mergedMin.x, box.min.x);
    mergedMin.y = Math.min(mergedMin.y, box.min.y);
    mergedMin.z = Math.min(mergedMin.z, box.min.z);
    // 更新最大点
    mergedMax.x = Math.max(mergedMax.x, box.max.x);
    mergedMax.y = Math.max(mergedMax.y, box.max.y);
    mergedMax.z = Math.max(mergedMax.z, box.max.z);
  }
  
  return { min: mergedMin, max: mergedMax };
}

const mergedBox = mergeBoundingBoxes(boundingBoxes);
console.log("合并后的包围盒:", mergedBox);
```