# AI Agent 快速参考卡

## 🚨 核心规则 (必须遵守)

```
┌─────────────────────────────────────────────────────┐
│  🚨 MANDATORY: 修改代码后立即运行                   │
│                                                     │
│     pnpm lint --fix                                 │
│                                                     │
│  ✅ 自动修复 ESLint + Prettier 错误                  │
│  ✅ 确保代码格式一致                                │
│  ✅ NEVER 留下格式化错误                            │
└─────────────────────────────────────────────────────┘
```

---

## 📋 标准工作流程

```bash
# 1. 修改代码
# (AI Agent 进行修改)

# 2. 立即修复格式
pnpm lint --fix

# 3. 验证无错误
pnpm lint

# 4. 如有错误，手动修复后重复 step 2-3

# 5. ✅ 完成
```

---

## 🎨 Prettier 格式规则

| 规则 | 正确示例 | 错误示例 |
|------|---------|---------|
| **引号** | `'single'` | `"double"` ❌ |
| **分号** | `const x = 1;` | `const x = 1` ❌ |
| **缩进** | 2 空格 | Tab 或 4 空格 ❌ |
| **行宽** | < 100 字符 | > 100 字符 ❌ |

---

## ✅ 代码示例

### ✅ GOOD

```typescript
import { Component } from 'react';

export const MyComponent = () => {
  const value = 10;
  return <div>{value}</div>;
};
```

### ❌ BAD

```typescript
import { Component } from "react"

export const MyComponent = () => {
  const value = 10
  return <div>{value}</div>
}
```

---

## 🔧 项目命令

| 位置 | 命令 |
|------|------|
| `/apps/web/` | `pnpm lint --fix` |
| `/apps/console/` | `pnpm lint --fix` |
| `/packages/*/` | `pnpm lint --fix` |

---

## 📦 构建流程

```bash
# 修改 packages/** 后：

# 1. 修复格式
pnpm lint --fix

# 2. 构建 package
cd packages/<package-name>
pnpm build

# 3. 构建 web (验证)
cd ../../apps/web
pnpm build

# ✅ 完成
```

---

## ⚠️ 常见错误

### 错误 1: 双引号
```typescript
// ❌
import { foo } from "bar"

// ✅
import { foo } from 'bar';
```

### 错误 2: 缺分号
```typescript
// ❌
const x = 1

// ✅
const x = 1;
```

### 错误 3: 超长行
```typescript
// ❌
const result = veryLongFunction(param1, param2, param3, param4, param5);

// ✅
const result = veryLongFunction(
  param1,
  param2,
  param3,
  param4,
  param5
);
```

---

## ✅ 质量检查清单

任务完成前确认：

```
✓ 运行了 pnpm lint --fix
✓ pnpm lint 无错误
✓ 使用单引号
✓ 使用分号
✓ 2 空格缩进
✓ Import 格式正确
```

---

## 🚫 禁止行为

❌ 提交带 Prettier 错误的代码  
❌ 跳过 `pnpm lint --fix`  
❌ 使用双引号  
❌ 省略分号  
❌ 不一致的缩进  

---

## 💡 记住

> **Write code → `pnpm lint --fix` → Verify → Done**

---

**快速记忆口诀**：

```
修改代码后，立即 lint fix
单引号分号，不能忘记
两空格缩进，保持一致
格式错误零，任务完成
```

---

Author: xiaoweihsueh@gmail.com  
Date: October 11, 2025

