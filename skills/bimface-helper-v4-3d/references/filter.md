# 构件过滤条件（Filter Format）

## 使用约束与说明

- 过滤条件格式为 JSON 数组，用于 `model.getMatchIds({objectData})` 等方法的参数
- 过滤条件本身是同步的，但 `getMatchIds` 返回 Promise，必须异步处理
- 模型中可用的过滤字段取决于模型本身的属性结构，不同模型支持的字段可能不同
- 在使用过滤前建议先调用 `model.getComponentProperty(componentId)` 查看某个构件的完整属性，了解可用的过滤字段

---

## 过滤条件规则

- **过滤条件是一个数组**，数组中的每个元素代表一组条件
- **数组元素之间是 OR 关系**（满足任意一组即可匹配）
- **同一元素内的 key-value 对之间是 AND 关系**（必须全部满足才匹配）

```javascript
// 伪代码表达：
// [ {A AND B AND C}, {D AND E} ]  →  (A AND B AND C) OR (D AND E)
```

---

## 常用过滤字段

| 字段 | 类型 | 说明 |
|------|------|------|
| `categoryId` | 字符串 | 构件类别 ID，如 `"-2001340"` 表示墙 |
| `family` | 字符串 | 族名称 |
| `familyType` | 字符串 | 族类型 |
| `levelName` | 字符串 | 楼层名称，如 `"F01"`、`"标高 1"` |
| `specialty` | 字符串 | 专业，如 `"建筑"`、`"结构"` |

---

## 示例

### 基本过滤

```javascript
// 匹配 F01 楼层中类别为 -2001340（墙）的构件
const objectData = [
  { categoryId: "-2001340", levelName: "F01" }
];

model.getMatchIds({ objectData }).then((componentIds) => {
  console.log("匹配到的构件 ID:", componentIds);
});
```

### OR 条件 —— 匹配 F01 或 F02 楼层的所有构件

```javascript
const objectData = [
  { levelName: "F01" },
  { levelName: "F02" }
];

model.getMatchIds({ objectData }).then((componentIds) => {
  model.hideComponents({ ids: componentIds });
});
```

### AND + OR 组合 —— 匹配 F01 层的墙 或 F02 层的柱

```javascript
const objectData = [
  { categoryId: "-2001340", levelName: "F01" },  // F01 的墙
  { categoryId: "-2001280", levelName: "F02" }    // F02 的柱
];

model.getMatchIds({ objectData }).then((componentIds) => {
  const color = new Glodon.Bimface.Common.Graphics.Color(255, 0, 0, 0.8);
  model.overrideComponentColor({ ids: componentIds }, color);
});
```

---

## 如何发现可用的过滤字段

```javascript
// 通过获取某个构件的完整属性来了解可用的字段
model.getComponentProperty("某个构件ID").then((property) => {
  console.log(property);
  // 在控制台输出中查看 categoryId、family、levelName 等字段
});
```

> 不同模型（Revit、IFC 等）提供的属性字段可能不同，应以实际模型返回的属性为准。
