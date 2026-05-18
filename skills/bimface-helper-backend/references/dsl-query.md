# DSL 构件查询语法

## 接口地址
```
POST https://api.bimface.com/data/v2/query/elementIds
```

## 说明
基于 DSL 语法组合查询符合条件的构件 ID 列表。语法灵感来自 Elasticsearch Query DSL。

## 请求参数

### Query
| 参数名 | 类型 | 说明 |
|--------|------|------|
| includeOverrides | boolean | 是否查询修改后的属性值，默认 false |
| excludeInvisibles | boolean | 是否剔除无几何信息的构件，默认 false |
| paginationContextId | string | 分页上下文 ID |
| paginationSize | int32 | 分页大小 |

### Body
| 参数名 | 必填 | 类型 | 说明 |
|--------|------|------|------|
| targetType | 是 | string | 目标类型：`file` 或 `integration` |
| targetIds | 是 | string[] | 目标 ID 列表 |
| query | 是 | object | 查询条件 |

## query 条件类型
| 条件 | 说明 | 示例 |
|------|------|------|
| `match` | 精确匹配属性值 | `{"match": {"floor": "F2"}}` |
| `contain` | 模糊匹配（包含） | `{"contain": {"floor": "F"}}` |
| `in` | 多值精确匹配（并集） | `{"in": {"floor": ["F1","F2","F3"]}}` |
| `boolAnd` | 逻辑与，条件数组全部满足 | `{"boolAnd": [{"contain":{"floor":"F"}}, {"match":{"family":"标准层"}}]}` |
| `boolOr` | 逻辑或，条件数组任一满足 | `{"boolOr": [{"in":{"floor":["F1","F2","F3"]}}, {"match":{"family":"标准层"}}]}` |

## 请求示例
```json
{
  "targetType": "file",
  "targetIds": ["1938888813662976"],
  "query": {
    "contain": { "floor": "F2" }
  }
}
```

## 响应示例
```json
{
  "code": "success",
  "data": [
    {
      "targetId": "1938888813662976",
      "elementIds": ["259504", "259778", "260503"]
    }
  ]
}
```
