# API 接口

## /check/friend

场景：判断某个用户是否为当前用户的好友

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称   | 类型   | 参数说明                                              | 参数示例                               |
    | ---------- | ------ | ----------------------------------------------------- | -------------------------------------- |
    | username | string | 待判断用户名称 | "Jimmy" |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明          | 参数示例                          |
    | -------- | ------ | ----------------- | --------------------------------- |
    | result | boolean | 判断是否为好友 | true |
    | tag | string | 对好友的分组 | "Classmates" |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg  | string | 返回信息 | "They are not friends." |

## /group/&lt;groupID&gt;

场景：用于获取群聊基本信息

类型：GET

=== "请求头"

    使用 GET 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    本方法不需要提供任何请求体。

=== "响应参数"

    | 参数名称                                | 类型     | 参数说明             | 参数示例                                       |
    | --------------------------------------- | -------- | -------------------- | ---------------------------------------------- |
    | group | Group | 群聊基本信息 | |
    | code | number | 状态码：-1 表示错误；0 表示正常 | -1 |
    | msg     | string   | 返回信息 | "Group doesn't exist." |
    
    其中，`Group` 类型包括：
    ```json
    {
        groupID: string; // 群聊 ID
        groupname: string; // 群聊名称
        master: string; // 群主用户名称
        admins: string[]; // 管理员用户名称列表
        avatar: string; // 群头像
        createTime: Date; // 建群时间
        invite_check: boolean; // 邀请是否需要审核（默认为 true ）
        announcement: string; // 群公告
        memberList: string[]; // 群成员用户名称列表
    }
    ```

## /group/create

场景：用于多选好友创建群聊

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称 | 类型     | 参数说明           | 参数示例                       |
    | ------ | -------- | ------------------ | ------------------------------ |
    | usernameList | string[] | 待加入用户名称列表 | ["1111", "2222"] |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明             | 参数示例           |
    | -------- | ------ | -------------------- | ------------------ |
    | groupID | string | 群聊 ID | "111111111" |
    | groupname | string | 群聊名称 | "Group of aaa" |
    | avatar | string | 群聊头像 | |
    | code | number | 状态码：-1 表示错误；0 表示正常 | -1 |
    | msg | string | 返回信息 | "You must choose at least one group member." |

## /group/edit

场景：用于编辑群聊基本信息

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称           | 类型   | 参数说明   | 参数示例                               |
    | ------------------ | ------ | ---------- | -------------------------------------- |
    | groupID           | string | 群聊 ID    | "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" |
    | groupname         | string | 群聊昵称   | "test_group"                           |
    | announcement | string | 群公告     | "test_announcement"                    |
    | avatar | string | 群头像 |  |
    | invite_check | boolean | 邀请是否需要审核 | false |
    | master | string | 群主用户名称 | "hahaha" |
    | admins | string[] | 群管理员名称列表 | ["111", "222", "31fff"] |

=== "响应参数"

    ???+ danger
        这里前端从来没有用过 /group/edit 的返回值。可以考虑删去。
    
    | 参数名称 | 类型   | 参数说明             | 参数示例              |
    | -------- | ------ | -------------------- | --------------------- |
    | group | Group | 修改后的群聊基本信息 | |
    | code | number | 状态码：-1 表示错误；0 表示正常 | -1 |
    | msg   | string | 返回信息 | "You are not master, so you can't transfer master." |
    
    其中，`Group` 类型包括：
    ```json
    {
        groupID: string; // 群聊 ID
        groupname: string; // 群聊名称
        master: string; // 群主用户名称
        admins: string[]; // 管理员用户名称列表
        avatar: string; // 群头像
        createTime: Date; // 建群时间
        invite_check: boolean; // 邀请是否需要审核（默认为 true ）
        announcement: string; // 群公告
    }
    ```

## /list/conversation

???+ danger
    msg 需要打句号.。且不需要返回 username。

场景：用于获取当前用户参与所有会话的 ID

类型：GET

=== "请求头"

    使用 GET 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    本方法不需要提供任何请求体。

=== "响应参数"

    | 参数名称        | 类型                                   | 参数说明            | 参数示例                       |
    | --------------- | -------------------------------------- | ------------------- | ------------------------------ |
    | conversationIDs | string[] | 会话 ID 列表| |
    | avatars | string[] | 会话头像列表 | |
    | lastLoginTime | Date | 用户上次登录时间 | |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg | string   | 返回信息 | "Get conversation ids successfully." |

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
    | userList                            | User[] | 好友用户列表         |  |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg                                  | string   | 返回信息 | "Successfully fetched user friends." |
    
    ???+ danger
        这里有的类型有可能是 any。需要 export 一个 interface。
    
    其中，`User` 类型包括：
    ```json
    {
        username: string; // 用户名称
        nickname: string; // 用户昵称
        avatar: string; // 用户头像
        id: string; // 用户 ID
        tag: string; // 好友标签
        cursor: Date; // 用户最近查看时间
        friendCursor: Date; // 好友最近查看时间
    }
    ```

## /list/group

场景：用于获取群聊列表

类型：GET

=== "请求头"

    使用 GET 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    本方法不需要提供任何请求体。

=== "响应参数"

    | 参数名称                                | 类型     | 参数说明             | 参数示例                                       |
    | --------------------------------------- | -------- | -------------------- | ---------------------------------------------- |
    | groups | GroupInfo[] | 群聊列表        |  |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg | string   | 返回信息 | "Successfully fetched user groups." |
    
    其中，`GroupInfo` 类型包括：
    ```json
    {
        groupID: ObjectId; // 群聊 ID
        groupname: string; // 群聊名称
        avatar: string; // 群聊头像
    }
    ```

## /list/request

场景：用于获取所有申请

类型：GET

=== "请求头"

    使用 GET 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    本方法不需要提供任何请求体。

=== "响应参数"

    | 参数名称        | 类型                                   | 参数说明            | 参数示例                       |
    | --------------- | -------------------------------------- | ------------------- | ------------------------------ |
    | requests | RequestData[] | 由所有申请组成的列表 | |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg                                  | string   | 返回信息 | "Successfully fetched user requests." |
    
    其中，`RequestData` 类型包括：
    ```json
    {
        requestID: string; // 申请信息 ID
        username: string; // （好友申请）接收者
        groupID: string; // （群聊申请）群 ID
        groupname: string; // （群聊申请）群名称
        sender: string; // 申请发送者
        type: string; // 申请类型（"group" or "friend"）
        reason: string; // 申请语（"pending" or "accepted" 状态）/ 拒绝原因（"rejected" 状态）
        status: string; // 申请状态（"pending" or "accepted" or "rejected"）
        createTime: Date; // 申请时间
    }
    ```

## /login

场景：用于用户登录

类型：POST

=== "请求头"

    本方法不需要提供任何请求头。

=== "请求体"

    | 参数名称 | 类型   | 参数说明 | 参数示例    |
    | -------- | ------ | -------- | ----------- |
    | username | string | 用户名称 | "A1phaN"    |
    | password | string | 用户密码 | "IamA1phaN" |

=== "响应参数"

    | 参数名称    | 类型   | 参数说明                    | 参数示例                                 |
    | ----------- | ------ | --------------------------- | ---------------------------------------- |
    | token       | string | 若成功，返回⽤户 token      | "x-abcdefghijklmnopqrstuvwxyz"           |              |
    | code | number | 状态码：-2 表示 用户已注销；-1 表示出现其他错误；0 表示正常 | -2 |
    | msg      | string | 返回信息        | "User is a ghost." |

## /message

场景：获取某个会话所有聊天记录

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称   | 类型   | 参数说明                                              | 参数示例                               |
    | ---------- | ------ | ----------------------------------------------------- | -------------------------------------- |
    | convID | string | 会话 ID | "asdfasdf" |

=== "响应参数"

    ???+ danger
        这里的 msg 应修改为 `"Get full message list by convID successfully."`
    
    | 参数名称 | 类型   | 参数说明          | 参数示例                          |
    | -------- | ------ | ----------------- | --------------------------------- |
    | msgList | Message[] | 聊天记录列表 | |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg  | string | 返回信息 | "Get full message list by convID successfully." |
    
    其中，`Message` 类型包括：
    ```json
    {
        _id: string; // 消息 ID
        content: string;       // 消息内容
        sender: string;        // 发送者用户名
        createTime: Date;      // 消息创建时间
        refCount: number;      // 消息引用次数
        refMessage: BriefMsg;  // 消息引用消息
    }
    ```
    
    `BriefMsg` 类型是指：
    ```json
    interface BriefMsg {
        msgID: string; // 消息 ID
        content: string; // 消息内容
        sender: string; // 发送者用户名
    }
    ```

## /message/content

场景：获取某个会话中特定内容的聊天记录

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称   | 类型   | 参数说明                                              | 参数示例                               |
    | ---------- | ------ | ----------------------------------------------------- | -------------------------------------- |
    | convID | string | 会话 ID | "asdfasdf" |
    | content | string | 特定内容 | "aaaa" |

=== "响应参数"

    ???+ danger
        这里的 msg 应修改为 `"Get message list by content successfully."`
    
    | 参数名称 | 类型   | 参数说明          | 参数示例                          |
    | -------- | ------ | ----------------- | --------------------------------- |
    | msgList | Message[] | 聊天记录列表 | |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg  | string | 返回信息 | "Get message list by content successfully." |
    
    其中，`Message` 类型包括：
    ```json
    {
        _id: string; // 消息 ID
        content: string;       // 消息内容
        sender: string;        // 发送者用户名
        createTime: Date;      // 消息创建时间
        refCount: number;      // 消息引用次数
        refMessage: BriefMsg;  // 消息引用消息
    }
    ```
    
    `BriefMsg` 类型是指：
    ```json
    interface BriefMsg {
        msgID: string; // 消息 ID
        content: string; // 消息内容
        sender: string; // 发送者用户名
    }
    ```

## /message/delete

场景：对当前用户，删除（不展示）某个会话中特定的聊天记录

类型：DELETE

=== "请求头"

    使用 DELETE 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称   | 类型   | 参数说明                                              | 参数示例                               |
    | ---------- | ------ | ----------------------------------------------------- | -------------------------------------- |
    | convID | string | 会话 ID | "asdfasdf" |
    | msgID | string | 聊天记录 ID | "aaaa" |

=== "响应参数"

    ???+ danger
        这里后端写成了 "Delete massage successfully." 应为 message。
    
    | 参数名称 | 类型   | 参数说明          | 参数示例                          |
    | -------- | ------ | ----------------- | --------------------------------- |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg  | string | 返回信息 | "Delete message successfully." |

## /message/get?conversationID=&after=&limit=

场景：获取消息列表

类型：GET

=== "请求头"

    使用 GET 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    本方法不需要提供任何请求体。

=== "响应参数"

    | 参数名称 | 类型   | 参数说明          | 参数示例                          |
    | -------- | ------ | ----------------- | --------------------------------- |
    | messages | Message[] | 聊天记录列表 | |
    | has_next | boolean | 是否还有未返回的消息 | |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg  | string | 返回信息 | "Load messages successfully." |
    
    其中，`Message` 类型包括：
    ```json
    {
        conversation: string; // 会话 ID
        id: string; // 消息 ID
        content: string; // 消息内容
        sender: string; // 发送者用户名称
        createTime: Date; // 发送时间
    }
    ```

## /message/refer

场景：获取指定会话的某条信息引用数

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称   | 类型   | 参数说明                                              | 参数示例                               |
    | ---------- | ------ | ----------------------------------------------------- | -------------------------------------- |
    | convID | string | 会话 ID | "asdfasdf" |
    | msgID | string | 消息 ID | "aaaa" |

=== "响应参数"

    ???+ danger
        这里的 msg 应修改为 `"Get message reference count successfully."`。注意 get 的大写。
    
    | 参数名称 | 类型   | 参数说明          | 参数示例                          |
    | -------- | ------ | ----------------- | --------------------------------- |
    | refCount | number | 引用数 | 10 |
    | refMessage | BriefMsg | 引用消息 | |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg  | string | 返回信息 | "Get message reference count successfully." |
    
    其中，`Message` 类型包括：
    ```json
    {
        _id: string; // 消息 ID
        content: string;       // 消息内容
        sender: string;        // 发送者用户名
        createTime: Date;      // 消息创建时间
        refCount: number;      // 消息引用次数
        refMessage: BriefMsg;  // 消息引用消息
    }
    ```
    
    `BriefMsg` 类型是指：
    ```json
    interface BriefMsg {
        msgID: string; // 消息 ID
        content: string; // 消息内容
        sender: string; // 发送者用户名
    }
    ```

## /message/sender

场景：获取某个会话由某个用户发送的聊天记录

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称   | 类型   | 参数说明                                              | 参数示例                               |
    | ---------- | ------ | ----------------------------------------------------- | -------------------------------------- |
    | convID | string | 会话 ID | "asdfasdf" |
    | sender | string | 发送者用户名 | "Jimmy" |

=== "响应参数"

    ???+ danger
        这里的 msg 应修改为 `"Get message list by sender successfully."`
    
    | 参数名称 | 类型   | 参数说明          | 参数示例                          |
    | -------- | ------ | ----------------- | --------------------------------- |
    | msgList | Message[] | 聊天记录列表 | |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg  | string | 返回信息 | "Get message list by sender successfully." |
    
    其中，`Message` 类型包括：
    ```json
    {
        _id: string; // 消息 ID
        content: string;       // 消息内容
        sender: string;        // 发送者用户名
        createTime: Date;      // 消息创建时间
        refCount: number;      // 消息引用次数
        refMessage: BriefMsg;  // 消息引用消息
    }
    ```
    
    `BriefMsg` 类型是指：
    ```json
    interface BriefMsg {
        msgID: string; // 消息 ID
        content: string; // 消息内容
        sender: string; // 发送者用户名
    }
    ```

## /message/time

场景：获取某个会话在某个时间段的聊天记录

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    ???+ danger
        这里后端的筛选似乎是左闭右开区间？能否调整为闭区间？
    
    | 参数名称   | 类型   | 参数说明                                              | 参数示例                               |
    | ---------- | ------ | ----------------------------------------------------- | -------------------------------------- |
    | convID | string | 会话 ID | "asdfasdf" |
    | startTime | Date | 起始时间 | |
    | finishTime | Date | 结束时间 | |

=== "响应参数"

    ???+ danger
        这里的 msg 应修改为 `"Get message list by time successfully."`
    
    | 参数名称 | 类型   | 参数说明          | 参数示例                          |
    | -------- | ------ | ----------------- | --------------------------------- |
    | msgList | Message[] | 聊天记录列表 | |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg  | string | 返回信息 | "Get message list by time successfully." |
    
    其中，`Message` 类型包括：
    ```json
    {
        _id: string; // 消息 ID
        content: string;       // 消息内容
        sender: string;        // 发送者用户名
        createTime: Date;      // 消息创建时间
        refCount: number;      // 消息引用次数
        refMessage: BriefMsg;  // 消息引用消息
    }
    ```
    
    `BriefMsg` 类型是指：
    ```json
    interface BriefMsg {
        msgID: string; // 消息 ID
        content: string; // 消息内容
        sender: string; // 发送者用户名
    }
    ```

## /register

场景：用于用户注册

类型：POST

=== "请求头"

    本方法不需要提供任何请求头。

=== "请求体"
    
    | 参数名称 | 类型   | 参数说明 | 参数示例    |
    | -------- | ------ | -------- | ----------- |
    | username | string | 用户名称 | "A1phaN"    |
    | nickname | string | 用户昵称 | "111"       |
    | password | string | 用户密码 | "IamA1phaN" |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明             | 参数示例                      |
    | -------- | ------ | -------------------- | ----------------------------- |
    | code | number | 状态码：-1 表示错误；0 表示正常 | -1 |
    | msg   | string | 返回信息 | "User Already Exists." |

## /remove/friend

场景：删除好友

类型：DELETE

=== "请求头"

    使用 DELETE 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称 | 类型   | 参数说明   | 参数示例                               |
    | -------- | ------ | ---------- | -------------------------------------- |
    | username  | string | 好友用户名称 | "Jimmy" |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明             | 参数示例                 |
    | -------- | ------ | -------------------- | ------------------------ |
    | code | number | 状态码：-1 表示错误；0 表示正常 | -1 |
    | msg   | string | 返回信息 | "They are not friends." |

## /remove/group

场景：某用户退出群聊

类型：DELETE

=== "请求头"

    使用 DELETE 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    ???+ danger
        这里真的需要 master 吗？还是说，前端在退出群聊前 assert 群主不能为当前用户？
    
    | 参数名称   | 类型   | 参数说明       | 参数示例                               |
    | ---------- | ------ | -------------- | -------------------------------------- |
    | groupID    | string | 群聊 ID       | "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" |
    | master | string | 转让群主用户名 | "next-Master" |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明             | 参数示例                |
    | -------- | ------ | -------------------- | ----------------------- |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg | string | 若失败，返回错误信息 | "Exit from group successfully." |

## /remove/group/member

场景：使某用户退出群聊

类型：DELETE

=== "请求头"

    使用 DELETE 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称   | 类型   | 参数说明       | 参数示例                               |
    | ---------- | ------ | -------------- | -------------------------------------- |
    | groupID    | string | 群聊 id        | "aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" |
    | memberName | string | 待移除用户名称 | "Jimmy"                                |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明             | 参数示例                |
    | -------- | ------ | -------------------- | ----------------------- |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg      | string | 若失败，返回错误信息 | "Group does not exist." |

## /remove/user

场景：用于用户注销

类型：DELETE

=== "请求头"

    使用 DELETE 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    ???+ danger
        前端仍然在 body 里传入了 username。后端似乎没有用到？
    
    本方法不需要提供任何请求体。

=== "响应参数"

    | 参数名称 | 类型   | 参数说明             | 参数示例                                 |
    | -------- | ------ | -------------------- | ---------------------------------------- |
    | code | number | 状态码：-1 表示错误；0 表示正常 | -1 |
    | msg   | string | 返回信息 | "The user doesn't exist." |

## /request/accept

???+ danger
    @route 里不是 apply，应该是 accept。

场景：通过他人的好友请求/入群请求

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称   | 类型   | 参数说明                                              | 参数示例                               |
    | ---------- | ------ | ----------------------------------------------------- | -------------------------------------- |
    | requestID | string | 请求 ID（当返回群组和好友申请列表的时候会需要这个属性） | "1111" |
    | username        | string                                 | 被申请用户名称           |      "Jimmy"                          |
    | groupID | string | 被申请群聊 ID | "111"                    |                                |
    | sender | string | 申请人用户名称 | "Ubec" |
    | type            | string                                 | "group" or "friend" | "group"                        |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明          | 参数示例                          |
    | -------- | ------ | ----------------- | --------------------------------- |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg  | string |    返回信息               | "Application accept successfully." |

## /request/reject

场景：拒绝他人的好友请求/入群请求

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称   | 类型   | 参数说明                                              | 参数示例                               |
    | ---------- | ------ | ----------------------------------------------------- | -------------------------------------- |
    | requestID | string | 请求 ID（当返回群组和好友申请列表的时候会需要这个属性） | "1111" |
    | username        | string | 处理申请的用户名称 | "Jimmy" |
    | groupID | string | 被申请群聊 ID | "111" | |
    | type | string | "group" or "friend" | "group" |
    | reason | string | 拒绝理由 | "We're sorry." |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明          | 参数示例                          |
    | -------- | ------ | ----------------- | --------------------------------- |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg  | string |     返回信息              | "Application reject successfully." |

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
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg     | string |        返回信息                                               | "Application sent successfully." |

## /search/group

场景：查询特定群聊信息

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称 | 类型   | 参数说明   | 参数示例                       |
    | -------- | ------ | ---------- | ------------------------------ |
    | groupname | string | 群聊昵称   | "A1phaN"                       |

=== "响应参数"

    | 参数名称 | 类型     | 参数说明               | 参数示例                      |
    | --- | -------- | ---------------------- | ----------------------------- |
    | groupList | Group[] | 满足要求的群聊列表 |  |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg | string   | 返回信息   | "Get group lists successfully." |
    
    其中，`Group` 类型包括：
    ```json
    {
        groupID: ObjectId; // 群聊 ID
        groupname: string; // 群聊名称
        avatar: string; // 群聊头像
    }
    ```

## /search/group/member（已弃用）

???+ danger
    该接口已被弃用。

    场景：通过群成员昵称查询特定群聊成员用户名称
    
    类型：POST
    
    === "请求头"
    
        使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。
    
    === "请求体"
    
        | 参数名称 | 类型   | 参数说明   | 参数示例                       |
        | -------- | ------ | ---------- | ------------------------------ |
        | groupID | string | 群聊 ID   | "111231231414155"                       |
        | groupNickname | string | 群成员昵称 | "test111" |
    
    === "响应参数"
    
        | 参数名称                                | 类型     | 参数说明               | 参数示例                      |
        | --------------------------------------- | -------- | ---------------------- | ----------------------------- |
        | usernameList | string[] | 用户名称列表  | ["A1phaN", "Jimmy"]                       |
        | code | number | 状态码 | 0 |
        | msg       | string   | 若失败，返回错误信息   | "Such user does not exist." |

## /search/user

场景：查询特定用户信息

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"
    
    | 参数名称 | 类型   | 参数说明   | 参数示例                       |
    | -------- | ------ | ---------- | ------------------------------ |
    | nickname | string | 用户昵称   | "A1phaN"                       |

=== "响应参数"

    | 参数名称                                | 类型     | 参数说明               | 参数示例                      |
    | --------------------------------------- | -------- | ---------------------- | ----------------------------- |
    | userList | User[] | 满足要求的用户列表 |  |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg | string   | 返回信息   | "Friend found." |
    
    其中，`User` 类型包括：
    ```json
    {
        username: string; // 用户名称
        nickname: string; // 用户昵称
        avatar: string; // 用户头像
    }
    ```

## /tag

场景：修改当前用户对某个用户的分组

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称   | 类型   | 参数说明                                              | 参数示例                               |
    | ---------- | ------ | ----------------------------------------------------- | -------------------------------------- |
    | username | string | 好友名称 | "Jimmy" |
    | tag | string | 对该用户的分组名称 | "Family" |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明          | 参数示例                          |
    | -------- | ------ | ----------------- | --------------------------------- |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg  | string | 返回信息 | "Edit Tag successfully!" |

## /user/&lt;username&gt;

场景：用于获取用户信息

类型：GET

=== "请求头"

    使用 GET 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    本方法不需要提供任何请求体。

=== "响应参数"
    
    | 参数名称    | 类型   | 参数说明               | 参数示例                   |
    | ----------- | ------ | ---------------------- | -------------------------- |
    | user | User | 用户信息 | |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg | string | 返回信息 | "Successfully fetched user info." |
    
    其中，`User` 类型包括：
    ```json
    {
        username: string; // 用户名称
        nickname: string; // 用户昵称
        description: string; // 用户个人简介
        avatar: string; // 用户头像
        email: string; // 用户邮箱
        lastLoginTime: Date // 用户上次登录时间
        password: string; // 用户密码
        isOnline: boolean; // 用户是否在线
    }
    ```

## /user/edit

???+ danger
    修改成功不需要返回详细信息。

场景：用于修改个人信息

类型：POST

=== "请求头"

    使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。

=== "请求体"

    | 参数名称    | 类型   | 参数说明               | 参数示例                       |
    | ----------- | ------ | ---------------------- | ------------------------------ |
    | nickname    | string | 用户昵称              | "A1phaN"                   |
    | password | string | 用户密码 | "xxxx" |
    | email       | string | 用户邮箱                   | "deng@tsinghua.edu.cn"     |
    | description | string | 用户个人简介               | "My name is A1phaN."       |
    | avatar      | string   | 用户头像 |                            |
    | oldPassword | string | 原密码 | "xxx" |
    | newPassword | string | 新密码 | "yyy" |

=== "响应参数"

    | 参数名称 | 类型   | 参数说明             | 参数示例                   |
    | -------- | ------ | -------------------- | -------------------------- |
    | code | number | 状态码：-1 表示错误；0 表示正常 | 0 |
    | msg   | string | 返回信息 | "Password Reset Successfully." |

## /user/reset（已弃用）

???+ danger
    该接口已被弃用。

    场景：用于重置密码
    
    类型：POST
    
    === "请求头"
    
        使用 POST 方法请求该 API 时需要携带 JWT 令牌验证身份。请求头需要将 Authorization 字段设置为 JWT 令牌。
    
    === "请求体"
    
        | 参数名称     | 类型   | 参数说明   | 参数示例                       |
        | ------------ | ------ | ---------- | ------------------------------ |
        | token        | string | 用户 token | "x-abcdefghijklmnopqrstuvwxyz" |
        | old_password | string | 旧密码     | "IamA1phaN"                    |
        | new_password | string | 新密码     | "Iama1phan"                    |
    
    === "响应参数"
    
        | 参数名称 | 类型   | 参数说明             | 参数示例                      |
        | -------- | ------ | -------------------- | ----------------------------- |
        | code | number | 状态码 | 0 |
        | errmsg   | string | 若失败，返回错误信息 | "The password is too simple." |
