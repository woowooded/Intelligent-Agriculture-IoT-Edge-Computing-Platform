# 智慧教室物联网系统接口文档

## 1. 设备管理

### 1.1 设备列表查询

#### 1.1.1 基本信息

- **请求路径**：`/devices`  
- **请求方式**：`GET`  
- **接口描述**：该接口用于设备列表数据查询  

#### 1.1.2 请求参数

- **参数格式**：`queryString`  

| 参数名称      | 是否必须 | 示例             | 备注                         |
| ------------- | -------- | ---------------- | ---------------------------- |
| `page`        | 否       | `1`              | 页码，默认为 1               |
| `pageSize`    | 否       | `10`             | 每页数量，默认为 10          |
| `classroomId` | 否       | `classroom_101`  | 教室 ID                      |
| `type`        | 否       | `SENSOR`         | 设备类型                     |

**请求数据样例：**

```shell
/devices?page=1&pageSize=10&classroomId=classroom_101&type=SENSOR
```

#### 1.1.3 响应数据

- **参数格式**：`application/json`

| 参数名          | 类型     | 是否必须 | 备注                                        |
| --------------- | -------- | -------- | ------------------------------------------- |
| `code`          | number   | 必须     | 响应码，`1` 代表成功，`0` 代表失败          |
| `msg`           | string   | 非必须   | 提示信息                                    |
| `data`          | object   | 非必须   | 返回的数据                                  |
| `data.total`    | number   | 必须     | 总记录数                                    |
| `data.rows`     | object[] | 必须     | 设备列表                                    |
| `rows[].id`     | string   | 必须     | 设备 ID                                     |
| `rows[].name`   | string   | 必须     | 设备名称                                    |
| `rows[].type`   | string   | 必须     | 设备类型 (`SENSOR`, `ACTUATOR`, `GATEWAY`) |
| `rows[].status` | string   | 必须     | 设备状态 (`ONLINE`, `OFFLINE`, `ERROR`)    |
| `rows[].classroomId` | string | 必须  | 教室 ID                                     |
| `rows[].createTime`  | string | 必须  | 创建时间                                    |
| `rows[].updateTime`  | string | 必须  | 更新时间                                    |

**响应数据样例：**

```json
{
  "code": 1,
  "msg": "success",
  "data": {
    "total": 5,
    "rows": [
      {
        "id": "device_001",
        "name": "温度传感器-101教室",
        "type": "SENSOR",
        "status": "ONLINE",
        "classroomId": "classroom_101",
        "createTime": "2024-01-01T10:00:00",
        "updateTime": "2024-01-01T10:00:00"
      },
      {
        "id": "device_002",
        "name": "湿度传感器-101教室",
        "type": "SENSOR",
        "status": "ONLINE",
        "classroomId": "classroom_101",
        "createTime": "2024-01-01T10:00:00",
        "updateTime": "2024-01-01T10:00:00"
      }
    ]
  }
}
```

---

### 1.2 设备注册

#### 1.2.1 基本信息

- **请求路径**：`/devices`  
- **请求方式**：`POST`  
- **接口描述**：该接口用于注册新设备  

#### 1.2.2 请求参数

- **参数格式**：`application/json`

| 参数名        | 类型   | 是否必须 | 备注       |
| ------------- | ------ | -------- | ---------- |
| `name`        | string | 必须     | 设备名称   |
| `type`        | string | 必须     | 设备类型   |
| `classroomId` | string | 必须     | 教室 ID    |
| `config`      | object | 非必须   | 设备配置   |

**请求参数样例：**

```json
{
  "name": "温度传感器-102教室",
  "type": "SENSOR",
  "classroomId": "classroom_102",
  "config": {
    "samplingInterval": 30,
    "threshold": 28.5
  }
}
```

#### 1.2.3 响应数据

- **参数格式**：`application/json`

| 参数名 | 类型   | 是否必须 | 备注                               |
| ------ | ------ | -------- | ---------------------------------- |
| `code` | number | 必须     | 响应码，`1` 代表成功，`0` 代表失败 |
| `msg`  | string | 非必须   | 提示信息                           |
| `data` | object | 非必须   | 返回的数据                         |

**响应数据样例：**

```json
{
  "code": 1,
  "msg": "设备注册成功",
  "data": null
}
```

---

### 1.3 设备详情查询

#### 1.3.1 基本信息

- **请求路径**：`/devices/{id}`  
- **请求方式**：`GET`  
- **接口描述**：该接口用于根据 ID 查询设备详情  

#### 1.3.2 请求参数

- **参数格式**：路径参数

| 参数名 | 类型   | 是否必须 | 备注    |
| ------ | ------ | -------- | ------- |
| `id`   | string | 必须     | 设备 ID |

**请求参数样例：**

```text
/devices/device_001
```

#### 1.3.3 响应数据

- **参数格式**：`application/json`

| 参数名            | 类型   | 是否必须 | 备注     |
| ----------------- | ------ | -------- | -------- |
| `code`            | number | 必须     | 响应码   |
| `msg`             | string | 非必须   | 提示信息 |
| `data`            | object | 非必须   | 设备详情 |
| `data.id`         | string | 必须     | 设备 ID  |
| `data.name`       | string | 必须     | 设备名称 |
| `data.type`       | string | 必须     | 设备类型 |
| `data.status`     | string | 必须     | 设备状态 |
| `data.classroomId`| string | 必须     | 教室 ID  |
| `data.config`     | object | 非必须   | 设备配置 |
| `data.createTime` | string | 必须     | 创建时间 |
| `data.updateTime` | string | 必须     | 更新时间 |

**响应数据样例：**

```json
{
  "code": 1,
  "msg": "success",
  "data": {
    "id": "device_001",
    "name": "温度传感器-101教室",
    "type": "SENSOR",
    "status": "ONLINE",
    "classroomId": "classroom_101",
    "config": {
      "samplingInterval": 30,
      "threshold": 28.5
    },
    "createTime": "2024-01-01T10:00:00",
    "updateTime": "2024-01-01T10:00:00"
  }
}
```

---

### 1.4 更新设备信息

#### 1.4.1 基本信息

- **请求路径**：`/devices`  
- **请求方式**：`PUT`  
- **接口描述**：该接口用于更新设备信息  

#### 1.4.2 请求参数

- **参数格式**：`application/json`

| 参数名        | 类型   | 是否必须 | 备注     |
| ------------- | ------ | -------- | -------- |
| `id`          | string | 必须     | 设备 ID  |
| `name`        | string | 必须     | 设备名称 |
| `type`        | string | 必须     | 设备类型 |
| `classroomId` | string | 必须     | 教室 ID  |
| `config`      | object | 非必须   | 设备配置 |

**请求参数样例：**

```json
{
  "id": "device_001",
  "name": "温度传感器-101教室(更新)",
  "type": "SENSOR",
  "classroomId": "classroom_101",
  "config": {
    "samplingInterval": 60,
    "threshold": 30.0
  }
}
```

#### 1.4.3 响应数据

- **参数格式**：`application/json`

| 参数名 | 类型   | 是否必须 | 备注                               |
| ------ | ------ | -------- | ---------------------------------- |
| `code` | number | 必须     | 响应码，`1` 代表成功，`0` 代表失败 |
| `msg`  | string | 非必须   | 提示信息                           |
| `data` | object | 非必须   | 返回的数据                         |

**响应数据样例：**

```json
{
  "code": 1,
  "msg": "设备更新成功",
  "data": null
}
```

---

### 1.5 删除设备

#### 1.5.1 基本信息

- **请求路径**：`/devices/{id}`  
- **请求方式**：`DELETE`  
- **接口描述**：该接口用于删除设备  

#### 1.5.2 请求参数

- **参数格式**：路径参数

| 参数名 | 类型   | 是否必须 | 备注    |
| ------ | ------ | -------- | ------- |
| `id`   | string | 必须     | 设备 ID |

**请求参数样例：**

```text
/devices/device_001
```

#### 1.5.3 响应数据

- **参数格式**：`application/json`

| 参数名 | 类型   | 是否必须 | 备注                               |
| ------ | ------ | -------- | ---------------------------------- |
| `code` | number | 必须     | 响应码，`1` 代表成功，`0` 代表失败 |
| `msg`  | string | 非必须   | 提示信息                           |
| `data` | object | 非必须   | 返回的数据                         |

**响应数据样例：**

```json
{
  "code": 1,
  "msg": "设备删除成功",
  "data": null
}
```

---

## 2. 数据查询

### 2.1 传感器数据查询

#### 2.1.1 基本信息

- **请求路径**：`/sensor-data`  
- **请求方式**：`GET`  
- **接口描述**：该接口用于查询传感器数据  

#### 2.1.2 请求参数

- **参数格式**：`queryString`

| 参数名称    | 是否必须 | 示例                  | 备注         |
| ----------- | -------- | --------------------- | ------------ |
| `deviceId`  | 否       | `device_001`          | 设备 ID      |
| `startTime` | 否       | `2024-01-01T00:00:00` | 开始时间     |
| `endTime`   | 否       | `2024-01-02T00:00:00` | 结束时间     |
| `page`      | 否       | `1`                   | 页码，默认 1 |
| `pageSize`  | 否       | `10`                  | 每页数量，默认 10 |

**请求数据样例：**

```shell
/sensor-data?deviceId=device_001&startTime=2024-01-01T00:00:00&endTime=2024-01-02T00:00:00&page=1&pageSize=10
```

#### 2.1.3 响应数据

- **参数格式**：`application/json`

| 参数名              | 类型     | 是否必须 | 备注       |
| ------------------- | -------- | -------- | ---------- |
| `code`              | number   | 必须     | 响应码     |
| `msg`               | string   | 非必须   | 提示信息   |
| `data`              | object   | 非必须   | 返回的数据 |
| `data.total`        | number   | 必须     | 总记录数   |
| `data.rows`         | object[] | 必须     | 数据列表   |
| `rows[].id`         | string   | 必须     | 数据 ID    |
| `rows[].deviceId`   | string   | 必须     | 设备 ID    |
| `rows[].value`      | number   | 必须     | 传感器数值 |
| `rows[].unit`       | string   | 必须     | 数值单位   |
| `rows[].timestamp`  | string   | 必须     | 采集时间   |

**响应数据样例：**

```json
{
  "code": 1,
  "msg": "success",
  "data": {
    "total": 100,
    "rows": [
      {
        "id": "data_001",
        "deviceId": "device_001",
        "value": 25.6,
        "unit": "°C",
        "timestamp": "2024-01-01T10:00:00"
      },
      {
        "id": "data_002",
        "deviceId": "device_001",
        "value": 25.8,
        "unit": "°C",
        "timestamp": "2024-01-01T10:01:00"
      }
    ]
  }
}
```

---

## 3. 告警管理

### 3.1 告警规则列表查询

#### 3.1.1 基本信息

- **请求路径**：`/alerts/rules`  
- **请求方式**：`GET`  
- **接口描述**：该接口用于查询告警规则列表  

#### 3.1.2 请求参数

- 无

#### 3.1.3 响应数据

- **参数格式**：`application/json`

| 参数名              | 类型     | 是否必须 | 备注                                       |
| ------------------- | -------- | -------- | ------------------------------------------ |
| `code`              | number   | 必须     | 响应码，`1` 代表成功，`0` 代表失败         |
| `msg`               | string   | 非必须   | 提示信息                                   |
| `data`              | object[] | 非必须   | 告警规则列表                               |
| `data[].id`         | string   | 必须     | 规则 ID                                    |
| `data[].name`       | string   | 必须     | 规则名称                                   |
| `data[].condition`  | string   | 必须     | 条件 (`GREATER_THAN`, `LESS_THAN`, `EQUAL`) |
| `data[].threshold`  | number   | 必须     | 阈值                                       |
| `data[].deviceIds`  | string[] | 必须     | 设备 ID 列表                               |
| `data[].enabled`    | boolean  | 必须     | 是否启用                                   |
| `data[].createTime` | string   | 必须     | 创建时间                                   |

**响应数据样例：**

```json
{
  "code": 1,
  "msg": "success",
  "data": [
    {
      "id": "rule_001",
      "name": "高温告警",
      "condition": "GREATER_THAN",
      "threshold": 30.0,
      "deviceIds": ["device_001", "device_002"],
      "enabled": true,
      "createTime": "2024-01-01T10:00:00"
    },
    {
      "id": "rule_002",
      "name": "低温告警",
      "condition": "LESS_THAN",
      "threshold": 18.0,
      "deviceIds": ["device_001"],
      "enabled": true,
      "createTime": "2024-01-01T10:00:00"
    }
  ]
}
```

---

### 3.2 创建告警规则

#### 3.2.1 基本信息

- **请求路径**：`/alerts/rules`  
- **请求方式**：`POST`  
- **接口描述**：该接口用于创建告警规则  

#### 3.2.2 请求参数

- **参数格式**：`application/json`

| 参数名      | 类型     | 是否必须 | 备注         |
| ----------- | -------- | -------- | ------------ |
| `name`      | string   | 必须     | 规则名称     |
| `condition` | string   | 必须     | 条件         |
| `threshold` | number   | 必须     | 阈值         |
| `deviceIds` | string[] | 必须     | 设备 ID 列表 |
| `enabled`   | boolean  | 必须     | 是否启用     |

**请求参数样例：**

```json
{
  "name": "湿度过高告警",
  "condition": "GREATER_THAN",
  "threshold": 80.0,
  "deviceIds": ["device_003", "device_004"],
  "enabled": true
}
```

#### 3.2.3 响应数据

- **参数格式**：`application/json`

| 参数名 | 类型   | 是否必须 | 备注                               |
| ------ | ------ | -------- | ---------------------------------- |
| `code` | number | 必须     | 响应码，`1` 代表成功，`0` 代表失败 |
| `msg`  | string | 非必须   | 提示信息                           |
| `data` | object | 非必须   | 返回的数据                         |

**响应数据样例：**

```json
{
  "code": 1,
  "msg": "告警规则创建成功",
  "data": null
}
```

---

### 3.3 当前活跃告警查询

#### 3.3.1 基本信息

- **请求路径**：`/alerts/current`  
- **请求方式**：`GET`  
- **接口描述**：该接口用于查询当前活跃告警  

#### 3.3.2 请求参数

- 无

#### 3.3.3 响应数据

- **参数格式**：`application/json`

| 参数名              | 类型     | 是否必须 | 备注         |
| ------------------- | -------- | -------- | ------------ |
| `code`              | number   | 必须     | 响应码       |
| `msg`               | string   | 非必须   | 提示信息     |
| `data`              | object[] | 非必须   | 活跃告警列表 |
| `data[].id`         | string   | 必须     | 告警 ID      |
| `data[].ruleName`   | string   | 必须     | 规则名称     |
| `data[].deviceName` | string   | 必须     | 设备名称     |
| `data[].value`      | number   | 必须     | 触发值       |
| `data[].threshold`  | number   | 必须     | 阈值         |
| `data[].timestamp`  | string   | 必须     | 触发时间     |

**响应数据样例：**

```json
{
  "code": 1,
  "msg": "success",
  "data": [
    {
      "id": "alert_001",
      "ruleName": "高温告警",
      "deviceName": "温度传感器-101教室",
      "value": 31.5,
      "threshold": 30.0,
      "timestamp": "2024-01-01T14:30:00"
    }
  ]
}
```

---

## 4. 自动化控制

### 4.1 自动化规则列表查询

#### 4.1.1 基本信息

- **请求路径**：`/automation/rules`  
- **请求方式**：`GET`  
- **接口描述**：该接口用于查询自动化规则列表  

#### 4.1.2 请求参数

- 无

#### 4.1.3 响应数据

- **参数格式**：`application/json`

| 参数名              | 类型     | 是否必须 | 备注                               |
| ------------------- | -------- | -------- | ---------------------------------- |
| `code`              | number   | 必须     | 响应码，`1` 代表成功，`0` 代表失败 |
| `msg`               | string   | 非必须   | 提示信息                           |
| `data`              | object[] | 非必须   | 自动化规则列表                     |
| `data[].id`         | string   | -        | 规则 ID                            |
| `data[].name`       | string   | -        | 规则名称                           |
| `data[].condition`  | string   | -        | 条件                               |
| `data[].action`     | string   | -        | 动作                               |
| `data[].enabled`    | boolean  | -        | 是否启用                           |
| `data[].createTime` | string   | -        | 创建时间                           |

**响应数据样例：**

```json
{
  "code": 1,
  "msg": "success",
  "data": [
    {
      "id": "auto_001",
      "name": "自动调光",
      "condition": "光照度 < 300",
      "action": "打开灯光",
      "enabled": true,
      "createTime": "2024-01-01T10:00:00"
    }
  ]
}
```

---

## 5. 系统管理

### 5.1 教室列表查询

#### 5.1.1 基本信息

- **请求路径**：`/classrooms`  
- **请求方式**：`GET`  
- **接口描述**：该接口用于查询教室列表  

#### 5.1.2 请求参数

- 无

#### 5.1.3 响应数据

- **参数格式**：`application/json`

| 参数名                | 类型     | 是否必须 | 备注                               |
| --------------------- | -------- | -------- | ---------------------------------- |
| `code`                | number   | 必须     | 响应码，`1` 代表成功，`0` 代表失败 |
| `msg`                 | string   | 非必须   | 提示信息                           |
| `data`                | object[] | 非必须   | 教室列表                           |
| `data[].id`           | string   | 必须     | 教室 ID                            |
| `data[].name`         | string   | 必须     | 教室名称                           |
| `data[].location`     | string   | 必须     | 教室位置                           |
| `data[].deviceCount`  | number   | 必须     | 设备数量                           |

**响应数据样例：**

```json
{
  "code": 1,
  "msg": "success",
  "data": [
    {
      "id": "classroom_101",
      "name": "101教室",
      "location": "教学楼A栋1层",
      "deviceCount": 5
    },
    {
      "id": "classroom_102",
      "name": "102教室",
      "location": "教学楼A栋1层",
      "deviceCount": 3
    }
  ]
}
```
