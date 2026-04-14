# Hermes Agent 配置指南

本文档记录 Hermes Agent 的启动方式及阿里云百炼（DashScope）qwen3.5-plus 模型的配置方法。

---

## 一、项目启动

### 1. 激活虚拟环境

```bash
source venv/bin/activate
```

### 2. 启动 CLI

```bash
hermes              # 启动交互式 CLI
hermes setup        # 运行完整配置向导
hermes model        # 选择模型和 provider
hermes tools        # 配置工具
hermes doctor       # 诊断问题
```

### 3. 常用启动参数

```bash
hermes --help                           # 查看帮助
hermes --model qwen3.5-plus             # 指定模型
hermes --provider alibaba               # 指定 provider
```

---

## 二、配置阿里云百炼 qwen3.5-plus

### 1. 配置文件位置

| 文件 | 路径 | 用途 |
|------|------|------|
| API Keys | `~/.hermes/.env` | 存储密钥等敏感信息 |
| 配置文件 | `~/.hermes/config.yaml` | 存储模型、provider 等设置 |

### 2. 创建 .env 文件

```bash
cat > ~/.hermes/.env << 'EOF'
HERMES_MAX_ITERATIONS=90

# 阿里云百炼 API Key
DASHSCOPE_API_KEY=sk-your-api-key-here

# 国内版百炼 endpoint
DASHSCOPE_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
EOF
```

### 3. 修改 config.yaml

编辑 `~/.hermes/config.yaml`，设置模型：

```yaml
model:
  provider: alibaba
  default: qwen3.5-plus
```

或使用命令修改：

```bash
sed -i "s/model: ''/model:\n  provider: alibaba\n  default: qwen3.5-plus/" ~/.hermes/config.yaml
```

### 4. 国内版 vs 国际版 Endpoint

| 版本 | Base URL |
|------|----------|
| 国内版 | `https://dashscope.aliyuncs.com/compatible-mode/v1` |
| 国际版 | `https://dashscope-intl.aliyuncs.com/compatible-mode/v1` |

### 5. 获取百炼 API Key

访问阿里云百炼控制台：https://bailian.console.aliyun.com/

在「我的应用」→「API-KEY管理」中创建。

---

## 三、运行时切换模型

### CLI 中切换

```bash
# 切换到百炼的 qwen3.5-plus
/model alibaba:qwen3.5-plus

# 使用别名
/model dashscope:qwen3.5-plus

# 持久化保存到配置文件
/model alibaba:qwen3.5-plus --global
```

### 查看当前模型

```bash
/model
```

---

## 四、配置文件示例

### ~/.hermes/.env 完整示例

```bash
# 最大迭代次数
HERMES_MAX_ITERATIONS=90

# 阿里云百炼 API Key
DASHSCOPE_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxx

# 国内版百炼 endpoint
DASHSCOPE_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
```

### ~/.hermes/config.yaml 关键配置

```yaml
model:
  provider: alibaba
  default: qwen3.5-plus

providers: {}
fallback_providers: []
toolsets:
  - hermes-cli

agent:
  max_turns: 90
```

---

## 五、百炼支持的模型

```
qwen3.5-plus
qwen3-coder-plus
qwen3-coder-next
glm-5
glm-4.7
kimi-k2.5
MiniMax-M2.5
```

---

## 六、Hermes CLI 界面

### 启动后界面

```
╔══════════════════════════════════════════════════════════════╗
║  ⚕ NOUS HERMES - AI Agent Framework                         ║
║  Messenger of the Digital Gods          Nous Research        ║
╚══════════════════════════════════════════════════════════════╝

Model: qwen3.5-plus (alibaba)
Skills: 12 loaded
Sessions: 3 recent

> _  (等待输入)
```

### 常用交互命令

| 命令 | 作用 |
|------|------|
| `/help` | 显示帮助 |
| `/model` | 查看/切换模型 |
| `/new` | 开始新对话 |
| `/reset` | 重置对话 |
| `/tools` | 查看可用工具 |
| `/skills` | 浏览技能 |
| `/usage` | 查看使用统计 |
| `Ctrl+C` | 中断当前操作 |
| `Ctrl+D` | 退出 |

---

## 七、验证配置

### 查看 .env 文件

```bash
cat ~/.hermes/.env
```

### 查看 config.yaml 配置

```bash
head -5 ~/.hermes/config.yaml
```

### 查看隐藏文件

```bash
ls -la ~/.hermes/ | grep env
```

### 启动验证

```bash
hermes
# 输入 /model 确认显示: Current: qwen3.5-plus on alibaba
```

---

## 八、常见问题

### Q: .env 文件看不到？

`.env` 是隐藏文件，使用 `ls -la` 查看：

```bash
ls -la ~/.hermes/
```

### Q: 切换模型时报错 "no matching model"？

需要指定完整格式：

```bash
/model alibaba:qwen3.5-plus
```

### Q: 如何使用国内版百炼？

在 `.env` 中设置：

```bash
DASHSCOPE_BASE_URL=https://dashscope.aliyuncs.com/compatible-mode/v1
```

---

## 九、相关链接

- 百炼控制台: https://bailian.console.aliyun.com/
- Hermes 文档: https://hermes-agent.nousresearch.com/docs/
- GitHub: https://github.com/NousResearch/hermes-agent
- Discord: https://discord.gg/NousResearch
