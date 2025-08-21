# 如何使用修复后的项目

## 快速集成修复

如果您想要将修复应用到原项目，只需要：

### 方法1：替换文件
1. 下载修复后的文件：
   - `public/index.html` 
   - `public/app.js`

2. 替换原项目中对应的文件

3. 重启服务器

### 方法2：手动修复
1. 打开原项目的 `public/index.html`
2. 复制所有 `<script>` 标签内的JavaScript代码
3. 创建新文件 `public/app.js`，将代码粘贴进去
4. 删除HTML中的 `<script>` 标签
5. 在 `</body>` 前添加：`<script src="/app.js"></script>`
6. 在 `app.js` 末尾添加全局函数暴露代码

## 完整项目运行

要运行完整的WTF Cosmos JS项目：

1. **获取原项目源码**：
```bash
git clone https://github.com/Rexingleung/wtf-cosmos-js.git
cd wtf-cosmos-js
```

2. **应用修复**：
```bash
# 下载修复后的前端文件
curl -o public/index.html https://raw.githubusercontent.com/Rexingleung/wtf-cosmos-js-fixed/main/public/index.html
curl -o public/app.js https://raw.githubusercontent.com/Rexingleung/wtf-cosmos-js-fixed/main/public/app.js
```

3. **安装依赖并启动**：
```bash
npm install
npm start
```

4. **访问应用**：
打开浏览器访问 `http://localhost:3000`

## 验证修复成功

### ✅ 检查点
- [ ] 页面正常加载，无JavaScript错误
- [ ] 浏览器控制台无CSP违规警告
- [ ] 所有按钮功能正常
- [ ] 实时数据更新工作正常
- [ ] 表单提交功能正常

### 🔍 故障排除

**如果仍然有CSP错误**：
1. 确认 `app.js` 文件路径正确
2. 检查文件是否被正确引用
3. 验证服务器静态文件配置

**如果按钮不响应**：
1. 检查浏览器控制台的错误信息
2. 确认全局函数正确暴露到 `window` 对象

**如果数据不更新**：
1. 检查API端点是否正常工作
2. 验证定时器是否正确设置

## 联系支持

如果您在使用过程中遇到问题：
1. 查看 [CSP修复指南](./CSP_FIX_GUIDE.md)
2. 在项目Issues中提问
3. 参考原项目文档