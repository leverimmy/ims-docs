# Web API 接口

## /register

场景：用于用户注册

类型：POST

=== "请求体"

    ???+ warning
        这里加了 `nickname` 字段。
    
    | 参数名称 | 类型   | 参数说明 | 参数示例    |
    | -------- | ------ | -------- | ----------- |
    | username | string | 用户名称 | "A1phaN"    |
    | nickname | string | 用户昵称 | "111"       |
    | password | string | 用户密码 | "IamA1phaN" |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明             | 参数示例                      |
    | -------- | ------ | -------------------- | ----------------------------- |
    | code | int | 状态码 | 0 |
    | msg   | string | 若失败，返回错误信息 | "The password is too simple." |

## /login

场景：用于用户登录

类型：POST

=== "请求体"

    | 参数名称 | 类型   | 参数说明 | 参数示例    |
    | -------- | ------ | -------- | ----------- |
    | username | string | 用户名称 | "A1phaN"    |
    | password | string | 用户密码 | "IamA1phaN" |

=== "响应参数"

    | 参数名称    | 类型   | 参数说明                    | 参数示例                                 |
    | ----------- | ------ | --------------------------- | ---------------------------------------- |
    | token       | string | 若成功，返回⽤户 token      | "x-abcdefghijklmnopqrstuvwxyz"           |              |
    | code | int | 状态码 | 0 |
    | msg      | string | 若失败，返回错误信息        | "The username or password is incorrect." |

## /logout

场景：用于用户登出

类型：GET

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    本方法不需要提供任何请求体。

=== "响应参数"

    | 参数名称 | 类型   | 参数说明             | 参数示例                                 |
    | -------- | ------ | -------------------- | ---------------------------------------- |
    | code | int | 状态码 | 0 |
    | msg   | string | 若失败，返回错误信息 | "The username or password is incorrect." |

## /remove

场景：用于用户注销

类型：DELETE

=== "请求头"

    使用 DELETE 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    本方法不需要提供任何请求体。

=== "响应参数"

    | 参数名称 | 类型   | 参数说明             | 参数示例                                 |
    | -------- | ------ | -------------------- | ---------------------------------------- |
    | code | int | 状态码 | 0 |
    | msg   | string | 若失败，返回错误信息 | "The username or password is incorrect." |

## /user?username=

???+warning
    这里的 URL 并不需要后端操心。前端对 /user 发送 GET 请求。

场景：用于获取用户信息

类型：GET

=== "请求头"

    使用 DELETE 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称 | 类型   | 参数说明   | 参数示例                               |
    | -------- | ------ | ---------- | -------------------------------------- |
    | username     | string | 用户名称    | "Jimmy" |

=== "响应参数"

    ???+ warning
        这里的 lastLoginTime 的类型没有确定。
    
    | 参数名称    | 类型   | 参数说明               | 参数示例                   |
    | ----------- | ------ | ---------------------- | -------------------------- |
    | nickname    | string | 用户昵称              | "A1phaN"                   |
    | password | string | 用户密码 | "xxxx" |
    | email       | string | 邮箱                   | "deng@tsinghua.edu.cn"     |
    | lastLoginTime | string | 上次登录时间 | "2022-04-04 11:49:05" |
    | description | string | 个人简介               | "My name is A1phaN."       |
    | avatar      | string   | 用户头像（base64） |                            |
    | code | int | 状态码 | 0 |
    | msg      | string | 若失败，返回错误信息   | "The username is invalid." |

## /user/edit

场景：用于修改个人信息

类型：POST

=== "请求头"

    使用 DELETE 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称    | 类型   | 参数说明               | 参数示例                       |
    | ----------- | ------ | ---------------------- | ------------------------------ |
    | nickname    | string | 用户昵称              | "A1phaN"                   |
    | password | string | 用户密码 | "xxxx" |
    | email       | string | 邮箱                   | "deng@tsinghua.edu.cn"     |
    | description | string | 个人简介               | "My name is A1phaN."       |
    | avatar      | string   | 用户头像（base64） |                            |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明             | 参数示例                   |
    | -------- | ------ | -------------------- | -------------------------- |
    | code | int | 状态码 | 0 |
    | msg   | string | 若失败，返回错误信息 | "The username is invalid." |

## user/reset

???+ danger
    该接口已被弃用。

场景：用于重置密码

类型：POST

=== "请求体"

    | 参数名称     | 类型   | 参数说明   | 参数示例                       |
    | ------------ | ------ | ---------- | ------------------------------ |
    | token        | string | 用户 token | "x-abcdefghijklmnopqrstuvwxyz" |
    | old_password | string | 旧密码     | "IamA1phaN"                    |
    | new_password | string | 新密码     | "Iama1phan"                    |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明             | 参数示例                      |
    | -------- | ------ | -------------------- | ----------------------------- |
    | code | int | 状态码 | 0 |
    | errmsg   | string | 若失败，返回错误信息 | "The password is too simple." |

## /group

???+ danger
    该接口仍未完工。

场景：用于获取群聊基本信息

类型：GET

=== "请求头"

    使用 DELETE 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "响应参数"

    | 参数名称                                | 类型     | 参数说明             | 参数示例                                       |
    | --------------------------------------- | -------- | -------------------- | ---------------------------------------------- |
    | group_id                                | string   | 群聊 id              | "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee"         |
    | group_name                              | string   | 群聊名称             | "test_group"                                   |
    | group_announcement                      | string   | 群公告               | "test_announcement"                            |
    | user_id_list                            | string[] | 群成员 id 列表       | [                                              |
    | "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee", |          |                      |                                                |
    | "aaaaaaaa-ffff-cccc-dddd-eeeeeeeeeeee"  |          |                      |                                                |
    | ]                                       |          |                      |                                                |
    | errmsg                                  | string   | 若失败，返回错误信息 | "Error occurred due to Internal Server Error." |

## /group/edit

???+ danger
    该接口仍未完工。

场景：用于编辑群聊基本信息

类型：POST

=== "请求体"

    | 参数名称           | 类型   | 参数说明   | 参数示例                               |
    | ------------------ | ------ | ---------- | -------------------------------------- |
    | token              | string | 用户 token | "x-abcdefghijklmnopqrstuvwxyz"         |
    | group_id           | string | 群聊 id    | "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" |
    | group_name         | string | 群聊名称   | "test_group"                           |
    | group_announcement | string | 群公告     | "test_announcement"                    |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明             | 参数示例              |
    | -------- | ------ | -------------------- | --------------------- |
    | errmsg   | string | 若失败，返回错误信息 | "Invalid group name." |

## /group/create

???+ danger
    该接口仍未完工。

场景：用于多选好友创建群聊

类型：POST

=== "请求体"

    | 参数名称                                | 类型     | 参数说明           | 参数示例                       |
    | --------------------------------------- | -------- | ------------------ | ------------------------------ |
    | token                                   | string   | 用户 token         | "x-abcdefghijklmnopqrstuvwxyz" |
    | user_id_list                            | string[] | 待加入用户 id 列表 | ["1111", "2222"] |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明             | 参数示例           |
    | -------- | ------ | -------------------- | ------------------ |
    | errmsg   | string | 若失败，返回错误信息 | "Too few friends." |

## /list/group

???+ danger
    该接口仍未完工。

场景：用于获取群聊列表

类型：GET

=== "响应参数"

| 参数名称                                | 类型     | 参数说明             | 参数示例                                       |
| --------------------------------------- | -------- | -------------------- | ---------------------------------------------- |
| group_id_list                           | string[] | 群聊 id 列表         | ["1111", "2222"] |
| errmsg                                  | string   | 若失败，返回错误信息 | "Error occurred due to Internal Server Error." |

## /list/friend

场景：用于获取好友列表

类型：GET

=== "请求头"

    使用 GET 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    本方法不需要提供任何请求体。

=== "响应参数"

    | 参数名称                                | 类型     | 参数说明             | 参数示例                                       |
    | --------------------------------------- | -------- | -------------------- | ---------------------------------------------- |
    | usernameList                            | string[] | 好友用户名称列表         | ["Jimmy", "LittleSeven", "Ubec"] |
    | code | int | 状态码 | 0 |
    | msg                                  | string   | 若失败，返回错误信息 | "Error occurred due to Internal Server Error." |

## /list/request/

???+ danger
    该接口仍未完工。

场景：用于获取所有申请

类型：GET

=== "响应参数"

    | 参数名称                                | 类型     | 参数说明             | 参数示例                                       |
    | --------------------------------------- | -------- | -------------------- | ---------------------------------------------- |
    | request_id_list                         | string[] | 请求 id 列表         | [                                              |
    | "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee", |          |                      |                                                |
    | "aaaaaaaa-ffff-cccc-dddd-eeeeeeeeeeee"  |          |                      |                                                |
    | ]                                       |          |                      |                                                |
    | errmsg                                  | string   | 若失败，返回错误信息 | "Error occurred due to Internal Server Error." |

## /remove/friend

场景：删除好友

类型：DELETE

=== "请求头"

    使用 DELETE 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称 | 类型   | 参数说明   | 参数示例                               |
    | -------- | ------ | ---------- | -------------------------------------- |
    | username  | string | 好友用户名称    | "Jimmy" |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明             | 参数示例                 |
    | -------- | ------ | -------------------- | ------------------------ |
    | code | int | 状态码 | 0 |
    | msg   | string | 若失败，返回错误信息 | "Such friend does not exist." |

## /remove/group

???+ danger
    该接口仍未完工。

场景：退出群聊

类型：DELETE

=== "请求体"

| 参数名称 | 类型   | 参数说明   | 参数示例                               |
| -------- | ------ | ---------- | -------------------------------------- |
| token    | string | 用户 token | "x-abcdefghijklmnopqrstuvwxyz"         |
| group_id | string | 群聊 id    | "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" |

=== "响应参数"

| 参数名称 | 类型   | 参数说明             | 参数示例                |
| -------- | ------ | -------------------- | ----------------------- |
| errmsg   | string | 若失败，返回错误信息 | "Group does not exist." |

## /search/user

场景：查询特定用户信息

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称 | 类型   | 参数说明   | 参数示例                       |
    | -------- | ------ | ---------- | ------------------------------ |
    | username | string | 用户名称   | "A1phaN"                       |

=== "响应参数"

    | 参数名称                                | 类型     | 参数说明               | 参数示例                      |
    | --------------------------------------- | -------- | ---------------------- | ----------------------------- |
    | username_list                            | string[] | 满足要求的用户名称列表 | ["A1phaN"] |
    | code | int | 状态码 | 0 |
    | msg                                  | string   | 若失败，返回错误信息   | "Such user does not exist." |

## /search/record

???+ danger
    该接口仍未完工。

场景：用于获取聊天记录

类型：POST

=== "请求体"

| 参数名称 | 类型   | 参数说明       | 参数示例                               |
| -------- | ------ | -------------- | -------------------------------------- |
| token    | string | 用户 token     | "x-abcdefghijklmnopqrstuvwxyz"         |
| group_id | string | 对应的 id      | "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" |
| type     | bool   | 0-群聊；1-好友 | 0                                      |

=== "响应参数"

| 参数名称 | 类型     | 参数说明             | 参数示例       |
| -------- | -------- | -------------------- | -------------- |
| msg_list | string[] | 消息记录列表         |                |
| errmsg   | string   | 若失败，返回错误信息 | "No such user" |

## /request/send

场景：发送好友请求/入群请求

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称        | 类型                                   | 参数说明            | 参数示例                       |
    | --------------- | -------------------------------------- | ------------------- | ------------------------------ |
    | username        | string                                 | 被申请用户名称           |      "Jimmy"                          |
    | groupID | string | 被申请群聊 ID | "111"                    |                                |
    | sender | string | 申请人用户名称 | "Ubec" |
    | type            | string                                 | "group" or "friend" | "group"                        |
    | reason         | string                                 | 申请语              | "Please let me join in!"       |

=== "响应参数"

    | 参数名称    | 类型   | 参数说明                                                     | 参数示例                         |
    | ----------- | ------ | ------------------------------------------------------------ | -------------------------------- |
    | requestID | string | 请求 ID | "1111" |
    | createTime | Date   | 申请发送成功时才返回创建时间，这个由数据库录入时生成再返还给前端 |                                  |
    | code        | int | 状态码                                          |                0                 |
    | msg     | string |        返回信息                                               | "Application sent successfully." |

## /request/apply

场景：通过他人的好友请求/入群请求

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称   | 类型   | 参数说明                                              | 参数示例                               |
    | ---------- | ------ | ----------------------------------------------------- | -------------------------------------- |
    | requestID | string | 请求 ID（当返回群组和好友申请列表的时候会需要这个属性） | "1111" |
    | username   | string | 被申请人用户名称                                          | "LittleSeven"                         |
    | groupID | string | 被申请群 ID | "1" |
    | sender | string | 申请人用户名称 | "Jimmy" |
    | type       | string | "group" or "friend"                                   | "group"                                |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明          | 参数示例                          |
    | -------- | ------ | ----------------- | --------------------------------- |
    | code     | int | 状态码 | 0                                  |
    | msg  | string |    返回信息               | "Application apply successfully." |

## /request/reject

场景：拒绝他人的好友请求/入群请求

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称   | 类型   | 参数说明                                              | 参数示例                               |
    | ---------- | ------ | ----------------------------------------------------- | -------------------------------------- |
    | requestID | string | 请求 ID（当返回群组和好友申请列表的时候会需要这个属性） | "1111" |
    | username   | string | 被申请人用户名称                                          | "LittleSeven"                         |
    | groupID | string | 被申请群 ID | "1" |
    | sender | string | 申请人用户名称 | "Jimmy" |
    | type       | string | "group" or "friend"                                   | "group"                                |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明          | 参数示例                          |
    | -------- | ------ | ----------------- | --------------------------------- |
    | reason | string | 拒绝理由 | "We're sorry." |
    | code     | int | 状态码 |  0                                 |
    | msg  | string |     返回信息              | "Application apply successfully." |

## /post/persistence

???+ danger
    该接口仍未完工。

场景：第一类发送消息：持久化

类型：POST

=== "请求体"

    | 参数名称                                | 类型     | 参数说明                     | 参数示例                       |
    | --------------------------------------- | -------- | ---------------------------- | ------------------------------ |
    | token                                   | string   | 用户 token                   | "x-abcdefghijklmnopqrstuvwxyz" |
    | group_id_list                           | string[] | 发往指定会话列表（支持群发） | [                              |
    | "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee", |          |                              |                                |
    | "aaaaaaaa-ffff-cccc-dddd-eeeeeeeeeeee"  |          |                              |                                |
    | ]                                       |          |                              |                                |
    | msg_list                                | string[] | 消息记录列表                 |                                |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明             | 参数示例                                   |
    | -------- | ------ | -------------------- | ------------------------------------------ |
    | errmsg   | string | 若失败，返回错误信息 | "Sending unsuccessfully due to Network Error." |

## /post/notify

## /post/send

## /fetch/sync

## /fetch/roam
