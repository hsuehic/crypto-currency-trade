# 移动端策略管理功能实现

## 概述

在 Flutter 移动应用中实现了完整的策略管理功能，包括策略列表展示和详情页面。

## 新增文件

### 1. 数据模型
**文件**: `apps/mobile/lib/models/strategy.dart`

包含两个主要模型类：

#### Strategy 类
表示交易策略的完整信息：
- 基本信息：id, name, description, type, status
- 交易配置：exchange, symbol, parameters
- 状态信息：errorMessage, lastExecutionTime
- 时间戳：createdAt, updatedAt

```dart
class Strategy {
  final int id;
  final String name;
  final String status; // 'active', 'stopped', 'paused', 'error'
  final String? symbol;
  final String? exchange;
  // ... 更多字段
  
  bool get isActive => status == 'active';
  bool get isStopped => status == 'stopped';
}
```

#### StrategyPnL 类
表示策略的盈亏数据：
- PnL 数据：totalPnl, realizedPnl, unrealizedPnl
- 订单统计：totalOrders, filledOrders

```dart
class StrategyPnL {
  final double totalPnl;
  final double realizedPnl;
  final double unrealizedPnl;
  final int totalOrders;
  final int filledOrders;
  
  bool get isProfitable => totalPnl > 0;
  bool get isLoss => totalPnl < 0;
}
```

### 2. 策略服务
**文件**: `apps/mobile/lib/services/strategy_service.dart`

提供与后端 API 交互的服务：

```dart
class StrategyService {
  // 获取策略列表
  Future<List<Strategy>> getStrategies({
    String? status,
    String? exchange,
  })
  
  // 获取单个策略
  Future<Strategy?> getStrategy(int id)
  
  // 获取所有策略的 PnL
  Future<List<StrategyPnL>> getStrategiesPnL()
  
  // 获取单个策略的 PnL
  Future<StrategyPnL?> getStrategyPnL(int strategyId)
  
  // 更新策略状态
  Future<bool> updateStrategyStatus(int id, String status)
  
  // 删除策略
  Future<bool> deleteStrategy(int id)
}
```

**API 端点映射**:
- `GET /api/strategies` - 获取策略列表
- `GET /api/strategies/:id` - 获取单个策略
- `GET /api/analytics/pnl` - 获取 PnL 数据
- `POST /api/strategies/:id/status` - 更新状态
- `DELETE /api/strategies/:id` - 删除策略

### 3. 策略列表页面
**文件**: `apps/mobile/lib/screens/strategy.dart`

#### 主要功能
- ✅ 显示所有策略的列表
- ✅ 实时加载策略和 PnL 数据
- ✅ 状态指示器（彩色圆点）
- ✅ PnL 颜色编码（绿色=盈利，红色=亏损）
- ✅ 点击卡片进入详情页
- ✅ 加载状态和错误处理
- ✅ iOS 风格的大标题 AppBar

#### UI 组件

**StrategyCard 卡片**:
```
┌─────────────────────────────────────┐
│ ● Strategy Name        [ACTIVE]     │
│                                      │
│ 🔄 BTC/USDT   🏦 binance            │
│ ──────────────────────────────────  │
│ Total PnL          Orders           │
│ +123.45            15 / 20          │
└─────────────────────────────────────┘
```

**状态颜色**:
- 🟢 Active (绿色)
- ⚪ Stopped (灰色)
- 🟠 Paused (橙色)
- 🔴 Error (红色)

**PnL 颜色**:
- 🟢 正数 (盈利)
- 🔴 负数 (亏损)
- ⚪ 零 (持平)

#### 空状态
当没有策略时显示友好的空状态界面：
```
      📊
   No strategies yet
Create your first strategy to get started
```

### 4. 策略详情页面
**文件**: `apps/mobile/lib/screens/strategy_detail.dart`

#### 主要功能
- ✅ 显示策略完整信息
- ✅ 性能指标卡片（PnL、订单统计）
- ✅ 策略详情（类型、交易所、交易对等）
- ✅ JSON 格式显示参数
- ✅ 启动/停止策略按钮
- ✅ 删除策略功能（仅停止状态）
- ✅ AppBar 返回按钮

#### 页面布局

**1. 头部卡片**（渐变背景）
```
┌─────────────────────────────────────┐
│ [Active]                          ● │
│                                      │
│ BTC Moving Average                   │
│ Short and long term MA crossover     │
└─────────────────────────────────────┘
```

**2. 性能卡片**
```
┌─────────────────────────────────────┐
│ Performance                          │
│                                      │
│ +150.00  +120.00  +30.00            │
│ Total PnL Realized Unrealized       │
│ ──────────────────────────────────  │
│   📋 20        ✓ 15                 │
│ Total Orders  Filled Orders         │
└─────────────────────────────────────┘
```

**3. 详情卡片**
```
┌─────────────────────────────────────┐
│ Strategy Details                     │
│                                      │
│ Type          Moving Average         │
│ Exchange      binance               │
│ Symbol        BTC/USDT              │
│ Created       2024-01-15 10:30      │
│ Updated       2024-01-20 15:45      │
│ Last Execution 2024-01-20 15:45     │
└─────────────────────────────────────┘
```

**4. 参数卡片**
```
┌─────────────────────────────────────┐
│ Parameters                           │
│                                      │
│ {                                    │
│   "fastPeriod": 12,                 │
│   "slowPeriod": 26,                 │
│   "threshold": 0.001                │
│ }                                    │
└─────────────────────────────────────┘
```

**5. 操作按钮**
```
┌─────────────────────────────────────┐
│   ▶ Start Strategy                  │
│      (或 ⏸ Stop Strategy)            │
└─────────────────────────────────────┘
```

#### 交互功能

**启动/停止策略**:
1. 点击按钮触发状态切换
2. 显示加载动画
3. 调用 API 更新状态
4. 刷新策略数据
5. 显示成功/失败 Toast

**删除策略**:
1. 仅在策略停止时可用
2. 显示确认对话框
3. 确认后调用 API 删除
4. 返回列表页面
5. 显示成功 Toast

## 导航流程

```
策略列表页 (StrategyScreen)
    │
    ├─ 点击策略卡片 ────> 策略详情页 (StrategyDetailScreen)
    │                      │
    │                      ├─ 点击返回按钮 ──┐
    │                      │                  │
    │                      └─ 启动/停止 ──────┘
    │                         (返回并刷新)
    │
    └─ 从详情页返回后自动刷新列表
```

## 样式和设计

### 颜色方案

**状态颜色**:
```dart
Colors.green   // Active
Colors.grey    // Stopped
Colors.orange  // Paused
Colors.red     // Error
```

**PnL 颜色**:
```dart
Colors.green   // 盈利 (> 0)
Colors.red     // 亏损 (< 0)
Colors.grey    // 持平 (= 0)
```

### 字体

- **策略名称**: 16-28px, Bold
- **状态标签**: 12-14px, Semi-bold
- **交易对**: 14px, Monospace (等宽字体)
- **PnL 数值**: 16-24px, Bold
- **说明文字**: 12-14px, Regular

### 间距

- 卡片间距: 12px
- 内部边距: 16-20px
- 元素间距: 8-12px
- 分组间距: 16-24px

### 圆角

- 卡片圆角: 12-16px
- 标签圆角: 12-20px
- 按钮圆角: 8-12px

## 错误处理

### 网络错误
```dart
try {
  // API 调用
} catch (e) {
  // 显示错误消息
  ScaffoldMessenger.of(context).showSnackBar(
    SnackBar(
      content: Text('Failed to load: $e'),
      backgroundColor: Colors.red,
    ),
  );
}
```

### 加载状态
- 列表加载: `CircularProgressIndicator`
- 按钮操作: 按钮内部小加载器
- 禁用状态: 操作期间禁用交互

### 空状态
- 无数据: 显示友好的空状态提示
- 有图标、标题和说明文字

## 数据流

```
1. 用户打开策略页面
   ↓
2. 同时加载策略列表和 PnL 数据
   ├─ GET /api/strategies
   └─ GET /api/analytics/pnl
   ↓
3. 合并数据，匹配 strategy ID
   ↓
4. 显示策略卡片
   ↓
5. 用户点击卡片
   ↓
6. 进入详情页，传递 Strategy 和 PnL
   ↓
7. 用户操作（启动/停止）
   ↓
8. POST /api/strategies/:id/status
   ↓
9. 重新获取策略数据
   ↓
10. 更新 UI
```

## 使用方法

### 前提条件
1. Web Manager API 正常运行
2. 已配置 API 基础 URL（`NetworkParameter.host`）
3. 用户已登录并有有效的会话

### 启动应用
```bash
cd apps/mobile
flutter run
```

### 测试功能
1. 导航到策略标签页
2. 查看策略列表
3. 点击任意策略查看详情
4. 尝试启动/停止策略
5. 查看 PnL 变化

## API 依赖

此实现依赖于以下后端 API：

1. **GET /api/strategies**
   - 返回策略列表
   - 支持 status 和 exchange 过滤

2. **GET /api/strategies/:id**
   - 返回单个策略详情

3. **GET /api/analytics/pnl**
   - 返回所有策略的 PnL 数据
   - 包含 strategies 数组

4. **POST /api/strategies/:id/status**
   - 更新策略状态
   - Body: `{ "status": "active" | "stopped" }`

5. **DELETE /api/strategies/:id**
   - 删除策略

## 未来改进建议

### 功能增强
1. **下拉刷新**: 添加 RefreshIndicator
2. **搜索和过滤**: 按名称、状态、交易对过滤
3. **排序**: 按 PnL、名称、状态排序
4. **实时更新**: WebSocket 实时推送 PnL 更新
5. **图表**: 添加 PnL 历史图表
6. **创建策略**: 移动端创建新策略功能
7. **编辑策略**: 修改策略参数
8. **批量操作**: 批量启动/停止

### UI/UX 改进
1. **骨架屏**: 加载时显示骨架屏而不是空白
2. **动画**: 添加页面切换和状态变化动画
3. **手势**: 滑动删除、长按显示菜单
4. **夜间模式**: 完善暗色主题支持
5. **横屏支持**: 优化横屏布局
6. **平板适配**: 使用 Master-Detail 布局

### 性能优化
1. **分页加载**: 策略过多时分页
2. **缓存**: 本地缓存策略数据
3. **增量更新**: 只更新变化的数据
4. **图片优化**: 优化图标和图片加载

## 测试建议

### 单元测试
```dart
test('Strategy model fromJson', () {
  final json = {...};
  final strategy = Strategy.fromJson(json);
  expect(strategy.name, 'Test Strategy');
});
```

### Widget 测试
```dart
testWidgets('StrategyCard displays correctly', (tester) async {
  await tester.pumpWidget(StrategyCard(...));
  expect(find.text('BTC/USDT'), findsOneWidget);
});
```

### 集成测试
1. 测试完整的导航流程
2. 测试 API 调用和数据展示
3. 测试错误处理

## 总结

✅ **完成的功能**:
- 策略列表展示
- 策略详情页面
- PnL 颜色编码
- 启动/停止功能
- 删除功能
- 错误处理
- 加载状态
- 空状态处理

🎨 **设计特点**:
- 现代化 Material Design
- 清晰的视觉层次
- 直观的颜色编码
- 流畅的导航体验
- iOS 风格的大标题

🚀 **可以开始使用了！**
移动端策略管理功能已完全实现并可以投入使用。

