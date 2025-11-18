# 智慧教室物联网平台 - 项目开发规范

## 📋 文档版本控制

| 版本 | 日期       | 修改内容     | 修改人   |
| ---- | ---------- | ------------ | -------- |
| v1.0 | 2024-11-18 | 初始版本创建 | 项目经理 |

---

## 1. 代码开发规范

### 1.1 命名规范

#### 后端命名规范（Java）

```java
// 包名：全小写，使用公司域名倒写
package com.smartclassroom.device.controller;

// 类名：大驼峰，名词
public class DeviceController {}

// 方法名：小驼峰，动词开头
public void registerDevice() {}

// 常量：全大写，下划线分隔
public static final int MAX_DEVICE_COUNT = 100;

// 变量：小驼峰，有意义的名词
private String deviceName;
```

#### 前端命名规范（TypeScript / Vue）

```ts
// 文件命名：kebab-case
device-list.vue
device-service.ts

// 组件名：PascalCase
DeviceList.vue

// 变量/方法：camelCase
const deviceList = ref([])
function getDeviceList() {}

// 常量：UPPER_CASE
const API_TIMEOUT = 30000
```

#### 数据库命名规范（SQL）

```sql
-- 表名：snake_case，复数形式
devices
sensor_data
alert_rules

-- 字段名：snake_case
device_name
created_at
is_active
```

### 1.2 代码结构规范

#### 后端分层规范

```text
controller/     # 控制层：参数校验、响应封装
service/        # 业务层：业务逻辑处理
repository/     # 数据层：数据库操作
model/          # 模型层：数据对象
  ├── entity/   # 实体类
  ├── dto/      # 数据传输对象
  └── enums/    # 枚举类
```

#### 前端组件规范

```text
components/
├── common/     # 通用组件
├── layout/     # 布局组件  
├── device/     # 设备相关组件
└── [module]/   # 业务模块组件
```

### 1.3 注释规范

#### Java 注释示例

```java
/**
 * 设备管理控制器
 * 
 * @author 开发者姓名
 * @version 1.0
 * @since 2024-11-18
 */
@RestController
@RequestMapping("/devices")
public class DeviceController {
    
    /**
     * 注册新设备
     * 
     * @param request 设备注册请求参数
     * @return 注册结果
     */
    @PostMapping
    public ApiResponse<Device> registerDevice(@RequestBody DeviceRegisterRequest request) {
        // 业务逻辑
    }
}
```

#### Vue 注释示例

```vue
<template>
  <!-- 设备列表组件 -->
  <div class="device-list">
    <!-- 搜索区域 -->
    <div class="search-area">
      <!-- 搜索框 -->
      <el-input v-model="searchKey" placeholder="输入设备名称搜索" />
    </div>
  </div>
</template>

<script setup lang="ts">
/**
 * 设备列表组件
 * 功能：显示设备列表，支持搜索和分页
 */

interface Device {
  id: string
  name: string
  status: string
}

// 响应式数据
const searchKey = ref('')
const deviceList = ref<Device[]>([])
</script>
```

---

## 2. Git 分支管理规范

### 2.1 分支策略

```text
main
  ↑
develop
  ↑
feature/     ← 功能分支
hotfix/      ← 紧急修复分支
release/     ← 发布分支
```

### 2.2 分支命名规范

```bash
# 功能分支
feature/device-management
feature/alert-system-v1

# 修复分支  
hotfix/fix-device-register-bug
hotfix/critical-security-fix

# 发布分支
release/v1.0.0
release/v1.1.0
```

### 2.3 Commit 消息规范

```text
<type>(<scope>): <subject>

<body>

<footer>
```

**类型说明：**

- `feat`: 新功能  
- `fix`: 修复 bug  
- `docs`: 文档更新  
- `style`: 代码格式调整  
- `refactor`: 重构代码  
- `test`: 测试相关  
- `chore`: 构建过程或辅助工具变动  

**示例：**

```text
feat(device): 添加设备注册功能

- 实现设备注册API接口
- 添加设备信息验证逻辑
- 编写单元测试用例

Closes #123
```

### 2.4 代码审查规范

审查清单：

- 代码符合编码规范  
- 有适当的单元测试  
- 代码逻辑清晰正确  
- 没有安全漏洞  
- 性能考虑充分  
- 文档更新完整  

---

## 3. API 设计规范

### 3.1 RESTful 接口规范

**请求方法示例**

```http
GET    /devices          # 获取设备列表
GET    /devices/{id}     # 获取设备详情
POST   /devices          # 创建设备
PUT    /devices/{id}     # 更新设备
DELETE /devices/{id}     # 删除设备
```

**响应格式规范**

```json
{
  "code": 200,
  "message": "操作成功",
  "data": {
    "id": "device_001",
    "name": "温度传感器"
  },
  "timestamp": "2024-01-01T10:00:00Z"
}
```

**状态码规范（示意）**

```text
200 - 成功
400 - 请求参数错误
401 - 未授权
403 - 禁止访问
404 - 资源不存在
500 - 服务器内部错误
```

### 3.2 接口版本管理

```text
/api/v1/devices
/api/v1/alerts
/api/v2/devices    # 重大版本更新
```

---

## 4. 数据库设计规范

### 4.1 表设计规范

```sql
-- 必须包含的字段
CREATE TABLE devices (
    id VARCHAR(32) PRIMARY KEY COMMENT '主键ID',
    name VARCHAR(100) NOT NULL COMMENT '设备名称',
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    created_by VARCHAR(50) COMMENT '创建人',
    updated_by VARCHAR(50) COMMENT '更新人',
    is_deleted TINYINT DEFAULT 0 COMMENT '删除标记:0-正常,1-删除'
) COMMENT '设备表';
```

### 4.2 索引规范

```sql
-- 主键索引
ALTER TABLE devices ADD PRIMARY KEY (id);

-- 唯一索引
ALTER TABLE devices ADD UNIQUE INDEX uk_device_name (name);

-- 普通索引
ALTER TABLE devices ADD INDEX idx_status (status);
ALTER TABLE devices ADD INDEX idx_created_at (created_at);
```

---

## 5. 测试规范

### 5.1 单元测试规范

#### 后端单元测试示例（Java）

```java
@Test
@DisplayName("设备注册 - 成功案例")
public void testRegisterDevice_Success() {
    // Given
    DeviceRegisterRequest request = new DeviceRegisterRequest();
    request.setName("测试设备");
    
    // When
    Device result = deviceService.registerDevice(request);
    
    // Then
    assertNotNull(result.getId());
    assertEquals("测试设备", result.getName());
}
```

#### 前端单元测试示例（TypeScript）

```ts
describe('DeviceList.vue', () => {
  it('应该正确渲染设备列表', async () => {
    // 准备
    const devices = [{ id: '1', name: '测试设备' }]
    
    // 执行
    const wrapper = mount(DeviceList, {
      props: { devices }
    })
    
    // 验证
    expect(wrapper.text()).toContain('测试设备')
  })
})
```

### 5.2 测试覆盖率要求

| 测试类型 | 覆盖率要求 | 备注               |
| -------- | ---------- | ------------------ |
| 单元测试 | ≥ 80%      | 核心业务逻辑必须覆盖 |
| 集成测试 | ≥ 70%      | 主要业务流程覆盖   |
| E2E 测试 | ≥ 50%      | 关键用户路径覆盖   |

---

## 6. 文档规范

### 6.1 文档目录结构

```text
docs/
├── requirements/     # 需求文档
│   ├── use-case/    # 用例文档
│   └── spec/        # 需求规格
├── design/          # 设计文档
│   ├── api/         # API设计
│   ├── database/    # 数据库设计
│   └── architecture/# 架构设计
├── development/     # 开发文档
│   ├── setup/       # 环境搭建
│   └── guide/       # 开发指南
└── deployment/      # 部署文档
    ├── manual/      # 部署手册
    └── operation/   # 运维手册
```

### 6.2 接口文档规范

```markdown
## 1.1 设备列表查询

### 基本信息
> 请求路径：/devices
> 请求方式：GET
> 接口描述：该接口用于设备列表数据查询

### 请求参数
| 参数名称 | 是否必须 | 示例 | 备注 |
|---------|----------|------|------|

### 响应数据
| 参数名 | 类型 | 是否必须 | 备注 |
|--------|------|----------|------|
```

---

## 7. 安全规范

### 7.1 数据安全

```yaml
# 密码加密
password_encryption: bcrypt

# 敏感信息加密
sensitive_data_encryption: AES-256

# API密钥管理
api_key_rotation: 90days
```

### 7.2 接口安全

```java
// JWT token验证
@PreAuthorize("hasRole('ADMIN')")
public void deleteDevice(String deviceId) {
    // 业务逻辑
}

// 参数验证
@Valid
public void registerDevice(@RequestBody DeviceRegisterRequest request) {
    // 业务逻辑
}
```

---

## 8. 性能规范

### 8.1 响应时间要求

| 操作类型 | 最大响应时间 | 备注           |
| -------- | ------------ | -------------- |
| 简单查询 | ≤ 200ms      | 单表查询       |
| 复杂查询 | ≤ 1000ms     | 多表关联查询   |
| 数据写入 | ≤ 500ms      | 包含业务逻辑   |
| 文件上传 | ≤ 3000ms     | 10MB 以内文件 |

### 8.2 数据库性能

```sql
-- 查询必须使用索引
EXPLAIN SELECT * FROM devices WHERE status = 'ONLINE';

-- 大批量操作分批次处理
UPDATE devices SET status = 'OFFLINE' 
WHERE id IN (SELECT id FROM devices WHERE created_at < '2024-01-01' LIMIT 1000);
```

---

## 9. 部署规范

### 9.1 环境配置

```yaml
# application-dev.yml
server:
  port: 8080
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/smart_classroom_dev
  redis:
    host: localhost

# application-prod.yml  
server:
  port: 8080
spring:
  datasource:
    url: jdbc:mysql://mysql-prod:3306/smart_classroom
  redis:
    host: redis-prod
```

### 9.2 容器化规范

```dockerfile
# 后端 Dockerfile
FROM openjdk:17-jdk-slim
COPY target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app.jar"]

# 前端 Dockerfile
FROM nginx:alpine
COPY dist/ /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```
