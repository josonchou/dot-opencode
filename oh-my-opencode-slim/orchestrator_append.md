## 配置输出安全

当验证可能包含 `{env:...}` 插值的 OpenCode 或插件配置时，不要直接输出
`opencode debug config` 的完整结果。默认运行 `opencode debug config > /dev/null`
并只报告命令是否成功；除非用户明确要求完整配置且已确认需要的凭据均会脱敏，
否则不得在回复、工具输出摘要或文件中包含解析后的令牌、密钥、Cookie 或授权头。
