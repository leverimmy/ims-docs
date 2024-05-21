# 系统基本要求

## 可用性 密码加密 & token验证

### 后端加密

```typescript
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

```typescript
/**
 * @summary 验证令牌
 * @param {string} token - 令牌
 */
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

## 避免socket直接传递的实现

### 前端添加 `auth=token` 字段

```typescript
const curSocket = io(BACKEND_URL, {
    transports: ["websocket"],
    auth: { token }
});
```

### 后端验证

```typescript
io.use(async (socket, next) => {
    const token = socket.handshake.auth.token;
    const verifyResult = await verifyToken(token);
    if (verifyResult) next();
    else next(new Error("Invalid token"));
});
```
## 消息未成功发送提醒

### 额外监听 `send_msg_successfully` 事件

```typescript
curSocket.on("send_msg_successfully", (messageId: string) => {
    setReceivedMsgIds((prev) => [...prev, messageId]);
});
```

```typescript
socket.on("private_chat", async (message: Message, ref: boolean) => {
    ...
    io.to(socket.id).emit("send_msg_successfully", message.id);
    ...
});

socket.on("group_chat", async (message: Message, ref: boolean) => {
    ...
    io.to(socket.id).emit("send_msg_successfully", message.id);
    ...
});

```

### 前端处理监听事件

```typescript
setTimeout(() => {
// 如果在10秒内没有找到刚发的消息，则认为发送失败
if (receivedMsgIds && receivedMsgIds.indexOf(msgID) === -1) {
    // 从本地数据库中找到刚发的消息
    const sendMessage = cachedMessagesRef.current.find((message) => message.id === msgID);
    // 更新发送状态，把这条消息的内容前加上 <发送失败>
    if (sendMessage) sendMessage.content = "<发送失败> " + sendMessage.content;
    console.log("发送消息失败");
}
}, 1000 * 10); // 10秒
}
```