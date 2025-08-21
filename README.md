# WTF Cosmos JS - Fixed Version

这是 [wtf-cosmos-js](https://github.com/Rexingleung/wtf-cosmos-js) 项目的修复版本，主要解决了 Content Security Policy (CSP) 内联脚本错误。

## 修复内容

### 🐛 问题描述
原项目在运行时出现以下错误：
```
Refused to execute inline script because it violates the following Content Security Policy directive: "script-src 'self'"
```

### ✅ 解决方案
1. **提取内联脚本**：将 `public/index.html` 中的所有内联 JavaScript 代码提取到外部文件 `public/app.js`
2. **更新HTML引用**：在HTML文件底部添加对外部脚本的引用 `<script src="/app.js"></script>`
3. **保持功能完整**：确保所有原有功能都正常工作，包括：
   - 区块链初始化
   - 钱包管理
   - 交易处理
   - 挖矿控制
   - 实时数据更新

### 📁 修改的文件
- `public/index.html` - 移除了所有内联脚本
- `public/app.js` - 新增的外部JavaScript文件，包含所有原有逻辑

### 🔒 安全性改进
- 符合严格的 Content Security Policy 要求
- 消除了内联脚本的安全隐患
- 保持了原有的 Helmet 安全配置

## 快速开始

1. 克隆项目：
```bash
git clone https://github.com/Rexingleung/wtf-cosmos-js-fixed.git
cd wtf-cosmos-js-fixed
```

2. 复制原项目的后端代码：
```bash
# 您需要从原项目复制以下目录和文件：
# - src/ (整个目录)
# - package.json
# - 其他配置文件
```

3. 安装依赖：
```bash
npm install
```

4. 启动服务：
```bash
npm start
```

## 技术细节

### CSP 配置
服务器使用的 Content Security Policy 配置如下：
```javascript
contentSecurityPolicy: {
  directives: {
    defaultSrc: ["'self'"],
    styleSrc: ["'self'", "'unsafe-inline'", 'https://cdnjs.cloudflare.com'],
    scriptSrc: ["'self'", 'https://cdnjs.cloudflare.com'],
    imgSrc: ["'self'", 'data:', 'https:'],
  },
}
```

### 修复要点
- `scriptSrc` 只允许 `'self'` 和 `https://cdnjs.cloudflare.com`
- 不允许 `'unsafe-inline'`，因此内联脚本被禁止
- 通过外部文件引用解决了这个限制

## 完整项目使用

要获得完整的可运行项目，请：

1. 从原项目复制所有后端文件
2. 使用此仓库中修复后的前端文件
3. 按照原项目的README进行配置和启动

## 相关链接

- [原项目](https://github.com/Rexingleung/wtf-cosmos-js)
- [MDN CSP文档](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CSP)
- [Helmet.js文档](https://helmetjs.github.io/)

## 许可证

MIT License - 与原项目保持一致