# BUG-001: 注册成功后页面空白

## 问题描述
用户在前端注册成功后，页面显示空白，无法正常跳转。

## 复现步骤
1. 访问 http://dashboard.trader-usa.com/login
2. 点击"注册"切换到注册模式
3. 输入用户名、邮箱、密码
4. 点击"注册"按钮
5. **预期：** 注册成功后自动登录并跳转到游戏页面
6. **实际：** 页面显示空白

## 根本原因

### 前后端 API 响应格式不一致

**后端返回：**
```json
POST /api/auth/register
Response: {"message":"注册成功"}
```

**前端期望：**
```typescript
interface RegisterResponse {
  token: string
  user: { id: string; username: string }
}
```

前端代码尝试访问 `response.token` 和 `response.user`，但后端只返回 `message`，导致：
1. `token` 为 `undefined`
2. localStorage 存储失败
3. 页面状态异常，显示空白

## 影响范围
- 所有新用户注册流程
- 用户无法正常进入游戏

## 优先级
🔴 **P0 - 阻塞**

## 建议修复方案

### 方案 A：修改后端（推荐）
修改注册 API 返回格式，与登录 API 保持一致：

```typescript
// 后端 auth.controller.ts
@Post('register')
async register(@Body() body: RegisterDto) {
  const user = await this.authService.register(body);
  const token = this.jwtService.sign({ userId: user.id, username: user.username });
  return {
    token,
    user: {
      id: user.id,
      username: user.username
    }
  };
}
```

### 方案 B：修改前端
修改前端 `api.ts` 和 `Login.tsx`，处理注册成功后自动调用登录：

```typescript
// api.ts
register: async (username: string, email: string, password: string) => {
  await request('/auth/register', { ... });
  // 注册成功后自动登录
  return api.login(username, password);
}
```

## 相关文件
- 后端：`/root/.openclaw/workspaces/devops/snake-game-repo/backend/src/auth/auth.controller.ts`
- 前端：`/root/.openclaw/workspaces/devops/snake-game-repo/frontend/src/services/api.ts`
- 前端：`/root/.openclaw/workspaces/devops/snake-game-repo/frontend/src/pages/Login.tsx`

## 测试验证
- [ ] 修复后注册成功返回 `{token, user}`
- [ ] 前端能正确解析响应
- [ ] localStorage 正确存储 token 和 user
- [ ] 注册后自动跳转到游戏页面

---
**报告人：** Bug 🐛
**报告时间：** 2026-03-05 09:45 GMT+8
**状态：** 🔴 待修复
