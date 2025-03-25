# 房间 API 文档

**Base URL**
`https://xxxxxxxxxxx/v2/klbq`

## 接口概览

1. [获取房间列表](#获取房间列表)

2. [提交房间数据](#提交房间数据)

## 通用鉴权

所有接口均需在 **URL参数** 携带鉴权凭证：

| 参数  | 类型  | 必填 | 说明                |
| --- | --- | -- | ----------------- |
| key | str | 是  | API 访问密钥，请联系管理员获取 |

示例：

`https://xxxxxxxxxxx/v2/klbq/station/pc/room_list?key=apikey`

## 获取房间列表

`/station/{room_type}/room-list`

请求方法: GET

### 路径参数

| 参数        | 类型  | 必填 | 说明                      |
| --------- | --- | -- | ----------------------- |
| room_type | str | 是  | 房间类型，可选 `pc` 或 `mobile` |

### 响应示例

```json
{
    "code": 0,
    "message": "success",
    "data": {
        "123456": {
            "time": 1742570635,
            "id": "123456",
            "description": "这是一段房间介绍",
            "tags": [
                "标签1",
                "标签2"
            ],
            "user_info": {
                "user_id": "123456789",
                "user_name": "name",
                "platform": "qq",
                "frequently_characters": [
                    "tag1",
                    "tag2"
                ]
            },
            "source_data": {
                "name": "123456789",
                "platform": "qq"
            }
        }
    }
}
```

#### 数据结构

| 字段      | 类型     | 说明             |
| ------- | ------ | -------------- |
| code    | int    | 状态码：0=成功，1=失败  |
| message | str    | 错误时的描述信息       |
| data    | object | 房间数据对象，键为房间 ID |

**房间对象字段**

| 字段          | 类型     | 说明                |
| ----------- | ------ | ----------------- |
| time        | int    | 房间最后更新时间（10位 时间戳） |
| room_id     | str    | 房间号               |
| description | str    | 房间描述              |
| tags        | str[]  | 房间标签列表            |
| user_info   | object | 用户数据对象            |
| source_info | object | 来源数据对象            |

**用户数据对象字段**

| 字段                    | 类型    | 必填 | 约束                  | 说明              |
| :-------------------- | :---- | :- | :------------------ | :-------------- |
| user_id               | str   | 是  | 长度 30位              | 用户id。如在q群则为qq号  |
| user_name             | str   | 否  | 长度 20位              | 用户昵称            |
| platform              | str   | 是  | 长度 10位              | 平台名称。如在q群则为`qq` |
| avatar                | str   | 否  | url                 | 暂不可用。头像url      |
| frequently_characters | str[] | 否  | 字符串列。超弦体小写英文名称，最多3个 | 用户常用超弦体         |

**来源数据对象字段**

| 字段   | 类型  | 说明   |
| :--- | :-- | :--- |
| name | str | 来源名称 |

## 提交房间数据

`/station/{room_type}/submit`

请求方法: POST

### 路径参数

| 参数        | 类型  | 必填 | 说明                      |
| --------- | --- | -- | ----------------------- |
| room_type | str | 是  | 房间类型，可选 `pc` 或 `mobile` |

#### 请求体

```json
{
    "room_data": {
        "room_id": "123456",
        "message": "房间描述",
        "tags": [
            "标签1",
            "标签2"
        ]
    }
}
```

| 字段        | 类型     | 必填 | 约束                  | 说明                |
| --------- | ------ | -- | ------------------- | ----------------- |
| room_id   | str    | 是  | 长度 6位               | 房间号               |
| tags      | str    | 是  | 最大 50 字符            | 房间描述              |
| user_info | object | 是  |                     | 用户数据对象，参考用户数据对象字段 |
| tags      | str[]  | 否  | 最多 5 个标签，每个标签最多6个字符 | 房间标签列表            |



### 响应示例

```json
{
    "code": 0,
    "message": "提交成功",
    "data": {}
}
```

