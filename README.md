### 背景

1. 想制作一些既美观简洁的软件
2. 需要足够轻量
3. 日常生活需要使用

### 功能

1. 创建待办事项：(标题、备注、详细信息、分组)
2. 设置提醒时间：(通知)
3. 设置重复时间：(重复通知)
4. 本地通知：利用Ionic Native提供的插件实现本地通知功能
5. 标记完成： (已完成、待完成、未开始)
6. 事项分类： (分组)
7. 事项状态统计： (全部、计划、今日(9/10)、完成)
8. 云同步：(Redis、MySQL)（待完成）
9. 白天夜晚主题

### plan-management 计划管理模块

```javascript
plan_info, group_info, priority_info, repeat_info
POST    /plan-management/users/<int:id>/plans          # 新建计划
{
	name          : string,
	remark        : string,
	groupName     : string,
	repeatName    : string,
	priorityName  : string,
	startDate     : string,
	endDate       : string,
}
PATCH   /plan-management/users/<int:id>/plans/<int:id> # 更新计划状态
{
	completed     : bool,
}
DELETE  /plan-management/users/<int:id>/plans/<int:id> # 删除计划
PATCH   /plan-management/users/<int:id>/plans/<int:id> # 更新计划设置
{
	name          : string,
	remark        : string,
	groupName     : string,
	repeatName    : string,
	priorityName  : string,
	startDate     : string,
	endDate       : string,
}
POST    /plan-management/users/<int:id>/groups   # 新建分组
{
	name   : string,
	remark : string,
}
DELETE  /plan-management/users/<int:id>/groups   # 删除分组
PATHCH  /plan-management/users/<int:id>/groups   # 更新分组
{
	name   : string,
	remark : string,
}
GET     /plan-management/users/<int:id>/plans?deleted=<bool>
GET     /plan-managements/users/<int:id>/plans?group-name=<string>
GET     /plan-management/users/<int:id>/plans?start-date=<string>&end-date=<string> # 查看时间段计划
GET     /plan-management/users/<int:id>/plans?completed=true&deleted=false
GET     /plan-management/users/<int:id>/plans?remark=<string>
```

### user-info 用户信息模块

```javascript
user_info
GET     /user-info/users/<int:id>
PATCH   /user-info/users/<int:id>/email
{
	email    : string,
}
PATCH   /user-info/users/<int:id>/password
{
	password : string,
}
PATCH   /user-info/users/<int:id>/avatar
{
	avatar   : file,
}
```

### auth 身份验证模块

```javascript
user_info
POST    /auth/login
{
	email    : string,
	password : string,
	avatar   : file,
}
POST    /auth/refresh
{
	refreshToken  : string,
}
POST    /auth/logout
{
	accessToken   : string,
}
POST    /auth/deactivate
{
	accessToken   : string,
}
POST    /auth/register
{
	email    : string,
	password : string,
	avatar   : file,
	verificationCode: string,
}
POST    /auth/verification-code/request
{
	email    : string,
}
POST    /auth/verification-code/verify
{
	verificationCode: string,
}
```

### 用户信息表 user_info

| **id** | **email** | **password_hash** | **avatar_url** | **activated** |
| ------------ | --------------- | ----------------------- | -------------------- | ------------------- |
| bigint       | varchar(64)     | varchar(256)            | varchar(64)          | tinyint             |

### 计划信息表 plan_info

| **id** | **name** | **remark** | **group_name** | **repeat_name** | **priority_name** | **start_date** | **end_date** | **completed** | **deleted** | **user_id** |
| ------------ | -------------- | ---------------- | -------------------- | --------------------- | ----------------------- | -------------------- | ------------------ | ------------------- | ----------------- | ----------------- |
| bigint       | varchar(64)    | varchar(64)      | varchar(64)          | varchar(64)           | varchar(64)             | date                 | date               | tinyint             | tinyint           | bigint            |

### 分组信息表 group_info

| **id** | **name** | **remark** | **user_id** |
| ------------ | -------------- | ---------------- | ----------------- |
| bigint       | varchar(64)    | varchar(64)      | bigint            |

### 计划等级表 priority_info

| **id** | **name** | **remark** |
| ------------ | -------------- | ---------------- |
| bigint       | varchar(64)    | varchar(64)      |

### 重复频率表 repeat_info

| **id** | **name** | **remark** |
| ------------ | -------------- | ---------------- |
| bigint       | varchar(64)    | varchar(64)      |

![1709530412765](image/README/1709530412765.png)
