# 开发指南

## 项目设置与启动

### 前置要求
- Node.js 18.0+
- npm 9.0+ 或 yarn 3.0+
- MongoDB 5.0+
- Redis 7.0+ (可选但推荐)
- Git

### 环境变量配置

#### 1. 后端环境变量
```bash
cd backend
cp .env.example .env
```

编辑 `.env` 文件，配置以下内容：
```
NODE_ENV=development
PORT=3000
MONGODB_URI=mongodb://localhost:27017/dongming-community
JWT_SECRET=your-dev-secret-key
```

#### 2. 前端环境变量
创建 `frontend/web/.env.local` 文件：
```
VITE_API_URL=http://localhost:3000/api
VITE_SOCKET_URL=http://localhost:3000
```

### 快速启动 (使用 Docker Compose)

```bash
# 启动所有服务 (MongoDB、Redis、后端、前端)
docker-compose up -d

# 查看日志
docker-compose logs -f

# 停止服务
docker-compose down
```

### 本地开发启动

#### 启动 MongoDB
```bash
# 使用 Docker
docker run -d -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  mongo:5.0

# 或使用本地 MongoDB
mongod
```

#### 启动后端
```bash
cd backend
npm install
npm run dev
# 后端运行在 http://localhost:3000
```

#### 启动前端
```bash
cd frontend/web
npm install
npm run dev
# 前端运行在 http://localhost:5173
```

---

## 代码规范

### 文件命名规范
- **文件夹**: 小写 + 连字符 (e.g., `user-service`)
- **文件**: 
  - 组件: PascalCase (e.g., `UserCard.tsx`)
  - 工具函数: camelCase (e.g., `formatDate.ts`)
  - 类型定义: PascalCase (e.g., `User.ts`)
  - 路由: camelCase (e.g., `userRoutes.ts`)

### 代码风格

#### 后端 (TypeScript + Express)
```typescript
// 路由定义
import express from 'express';

const router = express.Router();

router.post('/users', async (req, res) => {
  try {
    // 业务逻辑
    res.status(200).json({ code: 200, data: {} });
  } catch (error) {
    res.status(500).json({ code: 500, message: '服务器错误' });
  }
});

export default router;
```

#### 前端 (React + TypeScript)
```typescript
import React, { useState } from 'react';

interface UserProps {
  userId: string;
  onUpdate?: () => void;
}

const UserProfile: React.FC<UserProps> = ({ userId, onUpdate }) => {
  const [user, setUser] = useState(null);

  return <div>{/* JSX */}</div>;
};

export default UserProfile;
```

### 提交规范 (Conventional Commits)

```bash
# 格式
<type>(<scope>): <subject>

# 类型
feat:      新功能
fix:       修复bug
docs:      文档变更
style:     代码风格 (不改变代码含义)
refactor:  重构代码
test:      添加测试
chore:     构建过程、依赖管理等

# 示例
git commit -m "feat(chat): 添加消息已读功能"
git commit -m "fix(auth): 修复登录 token 过期问题"
git commit -m "docs: 更新 API 文档"
```

---

## 项目结构详解

### 后端结构
```
backend/
├── src/
│   ├── index.ts                 # 应用入口
│   ├── config/
│   │   ├── database.ts          # MongoDB 连接配置
│   │   ├── redis.ts             # Redis 连接配置
│   │   └── environment.ts       # 环境变量
│   ├── routes/
│   │   ├── auth.ts              # 认证路由
│   │   ├── users.ts             # 用户路由
│   │   ├── chats.ts             # 聊天路由
│   │   ├── posts.ts             # 动态路由
│   │   └── ads.ts               # 广告路由
│   ├── controllers/
│   │   ├── AuthController.ts
│   │   ├── UserController.ts
│   │   ├── ChatController.ts
│   │   ├── PostController.ts
│   │   └── AdController.ts
│   ├── models/
│   │   ├── User.ts
│   │   ├── Chat.ts
│   │   ├── Message.ts
│   │   ├── Post.ts
│   │   ├── Comment.ts
│   │   ├── Ad.ts
│   │   └── FriendRequest.ts
│   ├── services/
│   │   ├── AuthService.ts
│   │   ├── UserService.ts
│   │   ├── ChatService.ts
│   │   ├── PostService.ts
│   │   └── AdService.ts
│   ├── middleware/
│   │   ├── auth.ts              # JWT 验证
│   │   ├── errorHandler.ts      # 错误处理
│   │   └── validation.ts        # 参数验证
│   ├── socket/
│   │   ├── events.ts            # Socket.io 事件处理
│   │   └── handlers.ts          # 事件处理函数
│   ├── utils/
│   │   ├── logger.ts            # 日志
│   │   ├── helpers.ts           # 工具函数
│   │   └── validators.ts        # 验证函数
│   └── types/
│       └── index.ts             # 类型定义
├── tests/
├── Dockerfile
├── docker-compose.yml
├── .env.example
├── package.json
└── tsconfig.json
```

### 前端结构
```
frontend/web/
├── src/
│   ├── main.tsx                 # 应用入口
│   ├── App.tsx                  # 主应用组件
│   ├── pages/
│   │   ├── Home.tsx
│   │   ├── Chat.tsx
│   │   ├── Post.tsx
│   │   ├── Ad.tsx
│   │   ├── Profile.tsx
│   │   ├── Login.tsx
│   │   ├── Register.tsx
│   │   └── NotFound.tsx
│   ├── components/
│   │   ├── layout/
│   │   │   ├── Header.tsx
│   │   │   ├── Sidebar.tsx
│   │   │   └── Footer.tsx
│   │   ├── chat/
│   │   │   ├── ChatList.tsx
│   │   │   ├── ChatWindow.tsx
│   │   │   ├── MessageInput.tsx
│   │   │   └── FriendRequest.tsx
│   │   ├── post/
│   │   │   ├── PostCard.tsx
│   │   │   ├── PostForm.tsx
│   │   │   └── PostList.tsx
│   │   ├── ad/
│   │   │   ├── AdCard.tsx
│   │   │   ├── AdForm.tsx
│   │   │   └── AdFilter.tsx
│   │   └── common/
│   │       ├── Button.tsx
│   │       ├── Modal.tsx
│   │       └── Loading.tsx
│   ├── services/
│   │   ├── api.ts               # API 客户端
│   │   ├── auth.ts              # 认证服务
│   │   ├── user.ts              # 用户服务
│   │   ├── chat.ts              # 聊天服务
│   │   ├── post.ts              # 动态服务
│   │   ├── ad.ts                # 广告服务
│   │   └── socket.ts            # Socket.io 连接
│   ├── store/
│   │   ├── index.ts             # Store 配置
│   │   ├── slices/
│   │   │   ├── auth.ts
│   │   │   ├── user.ts
│   │   │   ├── chat.ts
│   │   │   ├── post.ts
│   │   │   └── ui.ts
│   │   └── hooks.ts             # Redux hooks
│   ├── utils/
│   │   ├── formatDate.ts
│   │   ├── formatNumber.ts
│   │   ├── validators.ts
│   │   └── storage.ts           # localStorage 管理
│   ├── types/
│   │   └── index.ts             # 类型定义
│   ├── styles/
│   │   ├── globals.css
│   │   ├── variables.css
│   │   └── components.css
│   └── router/
│       └── index.tsx            # 路由配置
├── public/
├── Dockerfile
├── .env.example
├── package.json
├── tsconfig.json
├── vite.config.ts
└── index.html
```

---

## 常见开发任务

### 添加新的 API 端点

#### 1. 创建模型 (backend/src/models)
```typescript
import mongoose from 'mongoose';

const userSchema = new mongoose.Schema({
  username: { type: String, required: true, unique: true },
  email: { type: String, required: true, unique: true },
  // ...其他字段
}, { timestamps: true });

export const User = mongoose.model('User', userSchema);
```

#### 2. 创建服务 (backend/src/services)
```typescript
export class UserService {
  async getUserById(userId: string) {
    return await User.findById(userId);
  }

  async updateUser(userId: string, data: any) {
    return await User.findByIdAndUpdate(userId, data, { new: true });
  }
}
```

#### 3. 创建控制器 (backend/src/controllers)
```typescript
export class UserController {
  async getUser(req: Request, res: Response) {
    const user = await this.userService.getUserById(req.params.userId);
    res.json({ code: 200, data: user });
  }
}
```

#### 4. 添加路由 (backend/src/routes)
```typescript
router.get('/users/:userId', UserController.getUser);
```

### 添加新的页面组件

#### 1. 创建页面 (frontend/src/pages)
```typescript
import React, { useEffect, useState } from 'react';

export const UserPage: React.FC = () => {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    // 获取数据
  }, []);

  return <div>{/* 页面内容 */}</div>;
};
```

#### 2. 创建组件 (frontend/src/components)
```typescript
import React from 'react';

interface UserCardProps {
  user: any;
  onSelect?: () => void;
}

export const UserCard: React.FC<UserCardProps> = ({ user, onSelect }) => {
  return <div onClick={onSelect}>{/* 卡片内容 */}</div>;
};
```

#### 3. 添加路由 (frontend/src/router)
```typescript
import { UserPage } from '@/pages/UserPage';

export const routes = [
  { path: '/users', element: <UserPage /> },
];
```

---

## 调试与测试

### 后端调试
```bash
# 使用 VS Code 调试
# 在 .vscode/launch.json 中配置

{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Backend",
      "program": "${workspaceFolder}/backend/src/index.ts",
      "outFiles": ["${workspaceFolder}/backend/dist/**/*.js"],
      "preLaunchTask": "npm: dev"
    }
  ]
}
```

### 前端调试
```bash
# 使用 React Developer Tools 浏览器扩展
# 使用 Redux DevTools 检查状态
```

### 测试
```bash
# 后端单元测试
cd backend
npm test

# 前端单元测试
cd frontend/web
npm test
```

---

## 部署指南

### 生产构建

#### 后端
```bash
cd backend
npm run build
NODE_ENV=production npm start
```

#### 前端
```bash
cd frontend/web
npm run build
# 将 dist 目录部署到静态服务器
```

### Docker 部署
```bash
# 构建镜像
docker build -t dongming-backend ./backend
docker build -t dongming-frontend ./frontend/web

# 运行容器
docker run -d -p 3000:3000 --name backend dongming-backend
docker run -d -p 5173:5173 --name frontend dongming-frontend
```

---

## 常见问题排查

### 数据库连接失败
```bash
# 检查 MongoDB 是否运行
mongosh --authenticationDatabase admin -u admin -p

# 检查连接字符串
mongodb://admin:password@localhost:27017/dongming-community?authSource=admin
```

### Socket.io 连接失败
```bash
# 检查 CORS 配置
# 确保前端 URL 在后端 CORS_ORIGIN 中

# 检查防火墙设置
sudo ufw allow 3000/tcp
```

### 前端 API 请求 404
```bash
# 确保后端正在运行
curl http://localhost:3000/api/health

# 检查 API 端点是否正确
# 检查 VITE_API_URL 环境变量
```

---

## 有用的链接

- [Express.js 文档](https://expressjs.com/)
- [Mongoose 文档](https://mongoosejs.com/)
- [React 文档](https://react.dev/)
- [Socket.io 文档](https://socket.io/docs/)
- [TypeScript 文档](https://www.typescriptlang.org/)

---

**最后更新**: 2026-06-08
