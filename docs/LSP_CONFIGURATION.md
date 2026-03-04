# OpenCode LSP 配置说明

## 配置概述

基于 [OpenCode 官方 LSP 文档](https://opencode.ai/docs/lsp/)，本配置文件已完善以下 LSP 服务器配置：

## 已配置的 LSP 服务器

### 1. TypeScript/JavaScript (`typescript`)
- **LSP 服务器**: `typescript-language-server`
- **功能**: 代码补全、语法检查、语义诊断、重构支持
- **配置特点**:
  - 相对路径导入模块偏好
  - 语法错误警告级别
  - 语义错误错误级别
  - 完整的代码补全支持（导入语句、代码片段等）

### 2. Python (`python`)
- **LSP 服务器**: `pyright`
- **功能**: 类型检查、代码补全、诊断、导入分析
- **配置特点**:
  - 基础类型检查模式
  - 工作区诊断模式
  - 自动导入补全
  - 虚拟环境支持
  - 自定义代码行长度 (120字符)

### 3. JSON (`json`)
- **功能**: JSON 模式验证、格式化、补全
- **配置特点**:
  - 模式请求检查
  - 模式缓存启用
  - 格式化设置 (2空格缩进)

### 4. CSS (`css`)
- **功能**: CSSLint 检查、代码补全
- **配置特点**:
  - 忽略 Tailwind CSS 相关规则
  - 属性值补全触发

### 5. HTML (`html`)
- **功能**: 自动闭合标签、格式化
- **配置特点**:
  - 自动关闭标签
  - 格式化设置 (2空格缩进, 120字符行长度)

### 6. Vue.js (`vue`)
- **功能**: Vue 单文件组件支持
- **配置特点**:
  - 使用 Prettier 格式化
  - Vue 特定格式设置

## 性能优化配置

```json
{
  "lspPerformance": {
    "diagnosticDebounceMs": 150,      // 诊断去抖动延迟
    "maxConcurrentDiagnostics": 5,    // 最大并发诊断请求数
    "diagnosticCacheTtl": 300,        // 诊断结果缓存时间(秒)
    "incrementalDiagnostics": true    // 增量诊断(性能优化)
  }
}
```

## 配置结构说明

每个 LSP 服务器支持以下属性：

| 属性 | 类型 | 描述 |
|------|------|------|
| `enabled` | boolean | 启用/禁用该 LSP 服务器 |
| `command` | string[] | 启动 LSP 服务器的命令 |
| `initializationOptions` | object | 初始化选项 |
| `env` | object | 环境变量 |

## 使用方法

### 1. 启动 OpenCode
```bash
cd /Users/joson/.config/opencode
opencode
```

### 2. 验证 LSP 服务器状态
在 OpenCode TUI 中运行：
```
/health
```

### 3. 查看 LSP 诊断
OpenCode 会自动：
- 检测文件类型
- 启动相应的 LSP 服务器
- 显示诊断信息
- 提供代码补全和跳转功能

## 常见问题

### Q: 如何禁用特定的 LSP 服务器？
取消注释对应服务器的 `enabled: false` 设置：
```json
"rust": {
  "enabled": false
}
```

### Q: 如何自定义 LSP 命令？
取消注释 `command` 属性：
```json
"typescript": {
  "command": ["custom-typescript-server", "--stdio"]
}
```

### Q: 如何添加自定义 LSP 服务器？
添加新的配置项：
```json
"custom-lsp": {
  "enabled": true,
  "command": ["custom-lsp-server", "--stdio"],
  "extensions": [".custom"]
}
```

### Q: 如何调试 LSP 问题？
1. 启用日志：
```json
"typescript": {
  "initializationOptions": {
    "log": "verbose"
  }
}
```
2. 或设置环境变量：
```json
"typescript": {
  "env": {
    "TSS_LOG": "-logVerbosity verbose -logFile tsserver.log"
  }
}
```

## 性能建议

1. **禁用不需要的 LSP 服务器** - 减少内存和 CPU 占用
2. **调整诊断去抖动时间** - 在 `lspPerformance.diagnosticDebounceMs` 中设置
3. **使用增量诊断** - 启用 `incrementalDiagnostics: true`
4. **限制并发诊断数** - 在 `lspPerformance.maxConcurrentDiagnostics` 中设置

## 相关文档

- [OpenCode 官方 LSP 文档](https://opencode.ai/docs/lsp/)
- [OpenCode 配置文档](https://opencode.ai/docs/config/)
- [OpenCode GitHub 仓库](https://github.com/anomalyco/opencode)

## 注意事项

1. 本配置已优化当前项目的开发需求（TypeScript/JavaScript 为主）
2. 所有配置项都提供了详细的中文注释
3. 敏感配置（如 API Key）已从注释中移除，请根据需要自行配置
4. 如需添加其他语言的 LSP 支持，请参考官方文档添加相应配置
