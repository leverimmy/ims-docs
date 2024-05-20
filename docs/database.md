# 数据库

我们项目采用的是 MongoDB 数据库。

![数据库](https://img.picgo.net/2024/05/20/database73d9199c88cba448.jpeg)

## MongoDB 数据库

这是 MongoDB 数据库与关系型数据库之间的差别：

![MongoDB 数据库](https://img.picgo.net/2024/05/20/mongodb4859b2e1634e463d.png)

## 1. 用户模块

### user 用户信息

| 字段            | 类型       | 约束          | 参数说明                |
| ------------- | -------- | ----------- | ------------------- |
| _id           | ObjectId | Primary Key | 用户标识符               |
| username      | string   |             | 用户名称（全局唯一）          |
| password      | string   |             | 用户密码（不少于 6 个字符）     |
| nickname      | string   |             | 用户昵称                |
| lastLoginTime | Date     |             | 最新登录时间              |
| description   | string   |             | 描述信息                |
| email         | string   |             | 用户邮箱                |
| avatar        | string   |             | 用户头像                |
| isGhost       | boolean  |             | 是否已注销（true 表示用户已注销） |

## 2. 好友模块

### friendship 好友关系

| 字段                 | 类型       | 约束          | 参数说明                  |
| ------------------ | -------- | ----------- | --------------------- |
| _id                | ObjectId | Primary Key | 好友关系 ID               |
| sender             | string   |             | 好友关系发起者用户名            |
| receiver           | string   |             | 好友关系接收者用户名            |
| createTime         | Date     |             | 好友关系创建时间              |
| senderTag          | string   |             | sender 被对方分为什么组       |
| receiverTag        | string   |             | receiver 被对方分为什么组     |
| senderDeleteList   | string[] |             | sender 的已删除消息 ID 列表   |
| receiverDeleteList | string[] |             | receiver 的已删除消息 ID 列表 |
| senderCursor       | Date     |             | 私聊中 sender 最新已读日期     |
| receiverCursor     | Date     |             | 私聊中 receiver 最新已读日期   |

### friendshipApplication 好友申请

| 字段            | 类型       | 约束          | 参数说明                                |
| ------------- | -------- | ----------- | ----------------------------------- |
| _id           | ObjectId | Primary Key | 好友申请 ID                             |
| sender        | string   |             | 好友申请发起者                             |
| receiver      | string   |             | 好友申请接收者                             |
| createTime    | Date     |             | 好友申请发送时间                            |
| status        | string   |             | 申请状态（pending, accepted or rejected） |
| message       | string   |             | 好友关系申请理由                            |
| reject_reason | string   |             | 拒绝理由                                |

## 3. 群组模块

###  group 群组基本信息
| 字段           | 类型       | 约束          | 参数说明                                                                |
| ------------ | -------- | ----------- | ------------------------------------------------------------------- |
| _id          | ObjectId | Primary Key | 群组 ID                                                               |
| master       | string   |             | 群主用户名                                                               |
| admins       | string[] |             | 管理员用户名列表                                                            |
| createTime   | Date     |             | 好友申请发送时间                                                            |
| announcement | string   |             | 群公告                                                                 |
| groupname    | string   |             | 群名称                                                                 |
| invite_check | boolean  |             | 是否仅能通过邀请入群（即是否不能被搜到）默认为 false ，表示既能被搜到又能通过邀请入群；true 则不能被搜到，仅能通过邀请入群 |

###  groupMember 群组成员绑定关系

| 字段             | 类型       | 约束          | 参数说明                           |
| -------------- | -------- | ----------- | ------------------------------ |
| _id            | ObjectId | Primary Key | 群组 ID                          |
| groupID        | ObjectId |             | 群组 ID                          |
| username       | string   |             | 用户名                            |
| group_nickname | string   |             | 群昵称                            |
| join_time      | Date     |             | 入群时间                           |
| role           | number   |             | 群角色<br>0：群主<br>1：管理员<br>2：普通成员 |
| cursor         | Date     |             | 群聊中该成员最新已读日期                   |
| deleteList     | string[] |             | 已删除消息 ID 列表                    |

###  groupApplication 入群申请

| 字段             | 类型       | 约束          | 参数说明                                |
| -------------- | -------- | ----------- | ----------------------------------- |
| _id            | ObjectId | Primary Key | 群组 ID                               |
| groupID        | ObjectId |             | 群组 ID                               |
| username       | string   |             | 用户名                                 |
| group_nickname | string   |             | 群昵称                                 |
| join_time      | Date     |             | 入群时间                                |
| status         | string   |             | 申请状态（pending, accepted or rejected） |
| message        | string   |             | 入群申请理由                              |
| reject_reason  | string   |             | 拒绝理由                                |

## 4. 消息系统

### convMessage 会话消息

| 字段        | 类型       | 约束          | 参数说明                         |
| --------- | -------- | ----------- | ---------------------------- |
| _id       | ObjectId | Primary Key | 会话消息 ID                      |
| conv_id   | ObjectId |             | 会话 ID（私聊就是好友关系 ID，群聊则是群组 ID） |
| msg_list  | Message  |             | 消息列表                         |


### Message 消息主体

**（一个接口，自定义数据类型）**

| 字段         | 类型               | 约束          | 参数说明   |
| ---------- | ---------------- | ----------- | ------ |
| _id        | string           | Primary Key | 消息 ID  |
| content    | string           |             | 消息内容   |
| sender     | string           |             | 发送者用户名 |
| createTime | Date             |             | 发送时间   |
| refCount   | number           |             | 消息引用次数 |
| refMessage | BriefMsg \| null |             | 所引用的消息 |

### BriefMsg 所引用的消息

**（一个接口，自定义数据类型）**

| 字段         | 类型               | 约束          | 参数说明   |
| ---------- | ---------------- | ----------- | ------ |
| msgID      | string           | Primary Key | 消息 ID  |
| content    | string           |             | 消息内容   |
| sender     | string           |             | 发送者用户名 |
