# Next.js Build Configuration Guide

本指南介绍如何使用不同的构建输出目录来防止构建产物相互覆盖。

## 🏗️ 构建目录配置

### 输出目录映射

| 构建模式 | 输出目录 | 用途 |
|---------|---------|------|
| 默认 | `.next` | 默认构建输出 |
| dev | `.next-dev` | 开发环境构建 |
| staging | `.next-staging` | 预发布环境构建 |
| prod | `.next-prod` | 生产环境构建 |

## 📝 可用命令

### 开发模式
```bash
# 开发服务器（使用 .next-dev 目录）
pnpm dev

# 清理后启动开发服务器
pnpm dev:clean
```

### 构建命令
```bash
# 默认构建（使用 .next 目录）
pnpm build

# 开发环境构建（使用 .next-dev 目录）
pnpm build:dev

# 预发布环境构建（使用 .next-staging 目录）
pnpm build:staging

# 生产环境构建（使用 .next-prod 目录）
pnpm build:prod
```

### 启动服务器
```bash
# 默认启动（端口 3002）
pnpm start

# 开发环境启动（端口 3002）
pnpm start:dev

# 预发布环境启动（端口 3003）
pnpm start:staging

# 生产环境启动（端口 3004）
pnpm start:prod
```

### 清理命令
```bash
# 清理所有构建目录
pnpm clean

# 清理特定环境的构建目录
pnpm clean:dev
pnpm clean:staging
pnpm clean:prod
```

## 🔧 工作流示例

### 场景1：并行开发和测试
```bash
# 终端1：开发环境
pnpm dev
# 应用运行在 http://localhost:3000，使用 .next-dev 目录

# 终端2：构建并测试预发布版本
pnpm build:staging
pnpm start:staging
# 应用运行在 http://localhost:3003，使用 .next-staging 目录
```

### 场景2：保持多个构建版本
```bash
# 构建所有环境版本
pnpm build:dev
pnpm build:staging  
pnpm build:prod

# 现在你有三个独立的构建输出：
# .next-dev/     - 开发版本
# .next-staging/ - 预发布版本
# .next-prod/    - 生产版本

# 可以随时启动任何版本
pnpm start:dev      # 端口 3002
pnpm start:staging  # 端口 3003
pnpm start:prod     # 端口 3004
```

### 场景3：CI/CD 流水线
```bash
# 在不同的 CI 阶段使用不同的构建命令
# 开发分支
BUILD_MODE=dev pnpm build

# 预发布分支
BUILD_MODE=staging pnpm build

# 生产分支
BUILD_MODE=prod pnpm build
```

## ⚙️ 环境变量

可以通过 `BUILD_MODE` 环境变量控制构建输出目录：

```bash
# 手动设置构建模式
BUILD_MODE=staging next build
BUILD_MODE=prod next dev
```

支持的 `BUILD_MODE` 值：
- `dev` - 使用 `.next-dev` 目录
- `staging` - 使用 `.next-staging` 目录  
- `prod` - 使用 `.next-prod` 目录
- 未设置 - 使用默认 `.next` 目录

## 📁 项目结构

```
apps/web/
├── .next/          # 默认构建输出
├── .next-dev/      # 开发环境构建
├── .next-staging/  # 预发布环境构建
├── .next-prod/     # 生产环境构建
├── next.config.ts  # Next.js 配置文件
└── package.json    # 包含所有构建脚本
```

## 🚀 优势

1. **防止覆盖**: 不同环境的构建输出分离，避免相互覆盖
2. **并行开发**: 可以同时运行开发和测试环境
3. **快速切换**: 预构建多个版本，快速启动任意环境
4. **CI/CD 友好**: 支持自动化部署流水线
5. **调试便利**: 保留所有版本的构建产物便于问题追踪

## 🔍 故障排除

### 端口冲突
如果遇到端口占用，可以手动指定端口：
```bash
BUILD_MODE=dev next start --port 3005
```

### 构建缓存问题
清理特定环境的构建缓存：
```bash
pnpm clean:dev
pnpm build:dev
```

### 环境变量不生效
确保在命令行中正确设置环境变量：
```bash
# Linux/macOS
BUILD_MODE=staging pnpm build

# Windows
set BUILD_MODE=staging && pnpm build
```

---

**Author**: xiaoweihsueh@gmail.com  
**Date**: December 10, 2024  
**Version**: 1.0
