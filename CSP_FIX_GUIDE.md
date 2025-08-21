# Content Security Policy 修复指南

## 问题分析

### 错误信息
```
Refused to execute inline script because it violates the following Content Security Policy directive: "script-src 'self'"
```

### 错误原因
原项目的 `public/index.html` 文件包含大量内联JavaScript代码，但服务器端的Content Security Policy (CSP) 配置禁止了内联脚本的执行。

### CSP 配置位置
在 `src/server.js` 文件中：

```javascript
app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'", 'https://cdnjs.cloudflare.com'],
      scriptSrc: ["'self'", 'https://cdnjs.cloudflare.com'],  // 注意这里没有 'unsafe-inline'
      imgSrc: ["'self'", 'data:', 'https:'],
    },
  },
}));
```

## 修复方案

### 方案选择
我们选择了**提取外部脚本**的方案，而不是简单添加 `'unsafe-inline'`，原因如下：

1. **安全性更高**：避免了内联脚本可能带来的XSS攻击风险
2. **代码维护性更好**：JavaScript代码独立管理，便于调试和更新
3. **符合最佳实践**：现代Web应用推荐将JS代码外部化

### 具体修改

#### 1. 创建外部JavaScript文件 (`public/app.js`)
```javascript
// 将所有原来在 <script> 标签内的代码移动到这里
// 包括：
// - 全局变量定义
// - 事件监听器
// - 所有功能函数
// - 全局函数暴露
```

#### 2. 更新HTML文件 (`public/index.html`)
```html
<!-- 删除所有 <script> 标签内的JavaScript代码 -->
<!-- 在 </body> 前添加外部脚本引用 -->
<script src="/app.js"></script>
```

### 关键技术点

#### 函数全局暴露
由于HTML中的 `onclick` 事件需要访问全局函数，需要显式暴露：

```javascript
// 在 app.js 底部添加
window.initializeBlockchain = initializeBlockchain;
window.showAPIDoc = showAPIDoc;
window.createWallet = createWallet;
window.checkBalance = checkBalance;
window.startMining = startMining;
window.stopMining = stopMining;
window.loadLatestBlocks = loadLatestBlocks;
```

#### 事件监听器兼容
DOM加载完成后的初始化逻辑保持不变：

```javascript
document.addEventListener('DOMContentLoaded', function() {
    loadStats();
    loadLatestBlocks();
    setupTransferForm();
    updateInterval = setInterval(loadStats, 5000);
});
```

## 验证修复效果

### 1. 检查浏览器控制台
修复后，浏览器控制台应该不再出现CSP相关错误。

### 2. 功能测试
确保以下功能正常工作：
- ✅ 页面加载和UI显示
- ✅ 数据实时更新（每5秒刷新）
- ✅ 按钮点击响应
- ✅ 表单提交
- ✅ API调用
- ✅ 通知显示

### 3. 网络面板检查
在开发者工具的Network面板中，确认 `app.js` 文件能正常加载。

## 安全性对比

| 方案 | 安全性 | 实现难度 | 维护性 |
|------|--------|----------|--------|
| 添加 'unsafe-inline' | ❌ 低 | ✅ 简单 | ❌ 差 |
| 外部脚本文件 | ✅ 高 | ⚠️ 中等 | ✅ 好 |
| Nonce 方案 | ✅ 高 | ❌ 复杂 | ⚠️ 中等 |

## 最佳实践建议

### 1. CSP 安全策略
- 尽量避免使用 `'unsafe-inline'`
- 优先使用外部文件或nonce机制
- 定期审查和更新CSP配置

### 2. 代码组织
- 将JavaScript代码模块化
- 使用现代ES6+语法和模块系统
- 考虑使用构建工具（webpack等）

### 3. 开发流程
- 在开发环境中启用严格的CSP
- 使用浏览器开发者工具监控CSP违规
- 定期进行安全测试

## 相关资源

- [MDN Content Security Policy](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CSP)
- [Helmet.js 文档](https://helmetjs.github.io/)
- [CSP 评估工具](https://csp-evaluator.withgoogle.com/)
