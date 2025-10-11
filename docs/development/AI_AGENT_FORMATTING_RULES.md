# AI Agent 代码格式化规则

## 📋 概述

本文档说明 AI Agent 在修改源代码时必须遵守的格式化和 Linting 规则。

## 🚨 核心原则

> **MANDATORY: 每次修改源代码后必须运行 `pnpm lint --fix`**
>
> **NEVER 提交或完成任务时留下格式化/Linting 错误**

---

## 🎯 强制规则

### 1. **立即修复 Linting 问题**

**规则**：修改任何源代码文件后，AI Agent **必须**立即执行 `pnpm lint --fix`

**原因**：
- ✅ 自动修复 ESLint 错误
- ✅ 应用 Prettier 格式化
- ✅ 修复 import 顺序
- ✅ 移除未使用的 imports
- ✅ 确保代码风格一致

**工作流程**：

```bash
# 1. 修改代码文件
# (AI Agent 进行修改)

# 2. 立即运行 lint fix
cd <project-directory>
pnpm lint --fix

# 3. 验证没有错误
pnpm lint

# 4. 如果仍有错误，手动修复
# (然后重新运行 step 2-3)
```

---

### 2. **遵循 Prettier 配置**

**规则**：所有新增或修改的代码必须遵循项目的 Prettier 配置

**关键格式规则**：

| 规则 | 设置 | 示例 |
|------|------|------|
| **引号风格** | 单引号 (`'`) | `import { foo } from 'bar';` |
| **分号** | 必需 (`;`) | `const x = 1;` |
| **尾随逗号** | ES5 风格 | `{ a, b, c }` 不是 `{ a, b, c, }` |
| **行宽** | 80-100 字符 | 自动换行 |
| **缩进** | 2 空格 | 一致的间距 |
| **箭头函数** | 需要括号时使用 | `(x) => x + 1` |

---

### 3. **编写代码时即遵循规范**

**规则**：AI Agent 在编写代码时就应该遵循格式规范，而不是依赖后期修复

**最佳实践**：

✅ **DO（推荐）**：

```typescript
// ✅ 正确：使用单引号，加分号
import { Component } from 'react';

export const MyComponent = () => {
  const value = 10;
  return <div>{value}</div>;
};
```

❌ **DON'T（避免）**：

```typescript
// ❌ 错误：双引号，缺分号
import { Component } from "react"

export const MyComponent = () => {
  const value = 10
  return <div>{value}</div>
}
```

---

## 📝 常见 Prettier 修复

### 1. 引号风格

```typescript
// ❌ BAD (双引号)
import { Component } from "react"

// ✅ GOOD (单引号)
import { Component } from 'react';
```

### 2. 分号

```typescript
// ❌ BAD (缺少分号)
const x = 1
const y = 2

// ✅ GOOD (带分号)
const x = 1;
const y = 2;
```

### 3. 长行处理

```typescript
// ❌ BAD (超出行宽)
const result = someVeryLongFunctionName(param1, param2, param3, param4, param5, param6);

// ✅ GOOD (自动换行)
const result = someVeryLongFunctionName(
  param1,
  param2,
  param3,
  param4,
  param5,
  param6
);
```

### 4. 对象/数组格式化

```typescript
// ❌ BAD (不一致的格式)
const obj = {a:1,b:2,c:3}

// ✅ GOOD (Prettier 格式化)
const obj = { a: 1, b: 2, c: 3 };

// 长对象：
const obj = {
  property1: 'value1',
  property2: 'value2',
  property3: 'value3',
};
```

### 5. Import 语句

```typescript
// ❌ BAD (错误的引号风格)
import {foo,bar,baz} from "module"

// ✅ GOOD (Prettier 格式化)
import { foo, bar, baz } from 'module';
```

---

## 🔧 AI Agent 工作流程

### 标准流程

```
┌─────────────────────────────────────────┐
│ 1. AI Agent 修改代码                    │
├─────────────────────────────────────────┤
│ 2. 立即运行: pnpm lint --fix           │
├─────────────────────────────────────────┤
│ 3. 检查结果: pnpm lint                 │
├─────────────────────────────────────────┤
│ 4. 如有错误，手动修复                   │
├─────────────────────────────────────────┤
│ 5. 重复 step 2-3 直到无错误             │
├─────────────────────────────────────────┤
│ 6. ✅ 完成任务                          │
└─────────────────────────────────────────┘
```

### 实际示例

```bash
# 场景: 修改了 apps/web/components/example.tsx

# Step 1: 进入项目目录
cd apps/web

# Step 2: 运行 lint fix
pnpm lint --fix

# 输出:
# ✔ No ESLint warnings or errors

# Step 3: 验证
pnpm lint

# 输出:
# ✔ No ESLint warnings or errors

# ✅ 完成！
```

---

## ⚠️ 错误处理

### 常见 Prettier 错误

```bash
# 错误示例:
./components/example.tsx
12:45  Error: Replace `"react"` with `'react';`  prettier/prettier
15:1   Error: Delete `··`  prettier/prettier
20:80  Error: Insert `⏎··`  prettier/prettier
```

### 解决步骤

1. **运行 `pnpm lint --fix`**（自动修复大多数问题）
2. **如果错误仍然存在，手动检查**：
   - 检查引号风格（单引号 vs 双引号）
   - 验证分号
   - 检查行长度
   - 验证缩进
3. **重新运行 `pnpm lint --fix`**
4. **验证 `pnpm lint`**

---

## 📊 项目特定命令

### Linting 命令

| 项目位置 | 命令 | 说明 |
|---------|------|------|
| `/apps/web/` | `pnpm lint --fix` | 格式化 web 应用代码 |
| `/apps/console/` | `pnpm lint --fix` | 格式化 console 应用代码 |
| `/packages/*/` | `pnpm lint --fix` | 格式化 package 代码 |
| 根目录 | `pnpm lint:all --fix` | 格式化整个 monorepo |

---

## ✅ 质量门槛

**任务完成前必须通过：**

- ✅ `pnpm lint` 输出中无 Prettier 错误
- ✅ 所有引号都是单引号 (`'`)
- ✅ 所有语句以分号结束 (`;`)
- ✅ 缩进一致（2 空格）
- ✅ 行长度合理（<100 字符）
- ✅ Imports 格式正确

---

## 🚫 禁止的行为

**AI Agent 绝对不能：**

❌ 提交带有 Prettier 错误的代码  
❌ 完成任务时留下格式化问题  
❌ 跳过 `pnpm lint --fix` 步骤  
❌ 忽略 linting 警告和错误  
❌ 使用双引号（应使用单引号）  
❌ 省略分号  
❌ 使用不一致的缩进  

---

## 🎯 检查清单

AI Agent 在完成任务前必须确认：

```markdown
- [ ] 所有修改的文件都运行了 `pnpm lint --fix`
- [ ] `pnpm lint` 没有报告任何错误
- [ ] 代码使用单引号
- [ ] 代码使用分号
- [ ] 缩进一致（2 空格）
- [ ] Import 语句格式正确
- [ ] 没有超长行（除非必要）
- [ ] 对象和数组格式化正确
```

---

## 📚 相关文档

- `.cursorrules` - AI Agent 完整规则
- `.prettierrc` - Prettier 配置文件
- `.eslintrc.json` - ESLint 配置文件

---

## 💡 为什么这很重要

### 代码一致性

✅ **统一的代码风格**：所有开发者和 AI Agent 遵循相同规范  
✅ **可读性更好**：一致的格式使代码更易理解  
✅ **减少冲突**：避免因格式问题导致的 Git 冲突  
✅ **专业性**：展示项目的高质量标准  

### 开发效率

✅ **自动化**：`pnpm lint --fix` 自动修复大多数问题  
✅ **快速反馈**：立即发现和修复格式问题  
✅ **减少 Review 时间**：不需要在 Code Review 中讨论格式  
✅ **构建成功**：避免因格式错误导致构建失败  

---

## 🎓 学习资源

### Prettier 官方文档
- [Prettier Options](https://prettier.io/docs/en/options.html)
- [Integrating with ESLint](https://prettier.io/docs/en/integrating-with-linters.html)

### ESLint 官方文档
- [ESLint Rules](https://eslint.org/docs/rules/)
- [ESLint with Prettier](https://github.com/prettier/eslint-plugin-prettier)

---

## 🔄 更新记录

| 日期 | 变更 | 说明 |
|------|------|------|
| 2025-10-11 | 添加 Prettier 规则 | 强制要求 AI Agent 遵循格式规范 |
| 2025-10-11 | 更新 Linting 工作流程 | 包含 Prettier 自动修复 |
| 2025-10-11 | 添加质量门槛 | 明确任务完成标准 |

---

## ✨ 总结

**核心信息**：

1. ✅ **每次修改代码后运行 `pnpm lint --fix`**
2. ✅ **使用单引号和分号**
3. ✅ **保持 2 空格缩进**
4. ✅ **永远不要留下格式化错误**

**记住**：良好的代码格式是专业软件开发的基础！🚀

---

Author: xiaoweihsueh@gmail.com  
Date: October 11, 2025

