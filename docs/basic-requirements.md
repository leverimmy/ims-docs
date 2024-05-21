# 系统基本要求

## 可用性 密码加密 & token 验证

### 后端加密

``` typescript title="im-backend/src/utils.ts" linenums="75" hl_lines="7-9"
/**
   * @summary 生成带盐值的哈希处理过的密码
   * @param {string} password - 用户注册密码
*/
export async function hashPassword(password: string) {
    const saltRounds = 10; // 设置盐值迭代次数，可根据需求调整
    // 使用 bcrypt 进行加密
    const hashedPassword = await bcrypt.hash(password, saltRounds);
    return hashedPassword;
}
```

### 后端解密

```typescript title="im-backend/src/utils.ts" linenums="5" hl_lines="6-8"
// 解析并验证刷新令牌
export async function verifyToken(token: string) {
    try {
        // 这里使用你的刷新令牌解码和验证逻辑
        const fastify = await initFastify();
        const decoded = fastify.jwt.verify(token);
        // 验证用户是否有效及令牌未被撤销等操作...
        return decoded;
    } catch (error) {
        throw new Error("Invalid refresh token");
    }
}
```

## 避免 socket 直接传递的实现

### 前端添加 `auth=token` 字段

``` typescript title="im-frontend/src/pages/_app.tsx" linenums="69" hl_lines="13-16"
        if(res.code === 0) {
                console.log("START INIT");
                const curUserCursor = await db.initConversations(token, username);
                console.log("CURUSERCURSOR", curUserCursor);
                if(updateUserCursor && curUserCursor) await updateUserCursor(curUserCursor);
                console.log("AFTER INIT", userCursor);
            }
        }
        init();
      }, [loginTime]);

    useEffect(() => {
        const curSocket = io(BACKEND_URL, {
            transports: ["websocket"],
            auth: { token }
        });
        if(username !== curUsername) {
            curSocket.off(); // 清除原有所有通信事件
            curSocket.emit("set username", username);
            setCurUsername(username);
        }

        curSocket.on("update_member_cursor", async (memberName: string, conversationID: string, cursor: string) => {
            const newCursor = userCursor;
```

### 后端验证

``` typescript title="im-backend/src/server.ts" linenums="149" hl_lines="6-11"
(async () => {
    const fastify = await initFastify();
    const httpServer: HttpServer = fastify.server;
    const io = new SocketIOServer(httpServer);

    io.use(async (socket, next) => {
        const token = socket.handshake.auth.token;
        const verifyResult = await verifyToken(token);
        if (verifyResult) next();
        else next(new Error("Invalid token"));
    });

    io.on("connection", (socket) => {
        socket.on("set username", async (username) => {
            addUser(username, socket.id);
            console.log(`Set ${socket.id}" username to ${username}`);
        });

        socket.on("join private_chat", (conversation) => {
```

## 消息未成功发送提醒

### 前端额外监听 `"send_msg_successfully"` 事件

``` typescript title="im-frontend/src/pages/_app.tsx" linenums="115" hl_lines="5-7"
            db.addMessage(frontendMes);
            dispatch(setLastUpdateTime(new Date().toString()));
        });

        curSocket.on("send_msg_successfully", (messageId: string) => {
            setReceivedMsgIds((prev) => [...prev, messageId]);
        });

        curSocket.on("group_chat", async (message) => {
            console.log("RECEIVE GROUP CHAT");
            const activeConversationId = db.activeConversationId;

```

### 后端处理监听事件

``` typescript title="ims-backend/src/server.ts" linenums="204" hl_lines="12 19"
                        io.to(groupID).emit("update_member_cursor", username, groupID, time);
                    });
                });

                socket.on("private_chat", async (message: Message, ref: boolean) => {
                    const sender = message.sender;
                    const senders = getSocketIdByUsername(sender);
                    const receiver = await friendshipModel.getFriendUsername(sender, message.conversation);
                    const recipients = getSocketIdByUsername(receiver);
                    const _id: string = message.id;
                    await PrivateChat(message, ref);
                    io.to(socket.id).emit("send_msg_successfully", message.id);
                    await Recipients(recipients, io, _id, sender, message);
                    await Senders(socket, senders, io, _id, sender, message);
                });

                socket.on("group_chat", async (message: Message, ref: boolean) => {
                    await GroupChat(message, ref);
                    io.to(socket.id).emit("send_msg_successfully", message.id);
                    socket.broadcast.emit("group_chat", message);
                });

                socket.on("disconnect", async () => {
                    const username = getUserBySocketId(socket.id);
```

### 前端处理监听事件

``` typescript title="im-frontend/src/components/conversations/Chatbox.tsx" linenums="113" hl_lines="13-22"
            setInputValue("");
            return;
        }
        // 按下Enter键时发送消息，除非同时按下了Shift或Ctrl
        if (!e.shiftKey && !e.ctrlKey) {
            e.preventDefault(); // 阻止默认事件
            e.stopPropagation(); // 阻止事件冒泡
            await db.clearUnreadCount(conversation.id);

            const msgID: string = sendMessage(inputValue, roomId, conversation.type); // 调用发送消息函数
            setInputValue("");

            setTimeout(() => {
                // 如果在10秒内没有找到刚发的消息，则认为发送失败
                if (receivedMsgIds && receivedMsgIds.indexOf(msgID) === -1) {
                    // 从本地数据库中找到刚发的消息
                    const curSendMessage = cachedMessagesRef.current.find((message) => message.id === msgID);
                    // 更新发送状态，把这条消息的内容前加上 <发送失败>
                    if (curSendMessage) curSendMessage.content = "<发送失败> " + curSendMessage.content;
                    console.log("发送消息失败");
                }
            }, 1000 * 10); // 10秒
        }
    };


    return (
        <div className={styles.container}>
```
