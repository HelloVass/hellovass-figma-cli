# figma-cli

[中文](README.md) | [English](README.en.md)

这是 `figma-cli` 的公开分发仓。

这里发布原生单文件二进制、SHA-256 checksum 和 CLI 使用说明。`figma-cli` 是确定性的 Figma REST 导出
命令行工具：读取设计稿 raw 节点，转换成平台无关的 ViewTree IR，并落盘截图、资源等可核验证据。
二进制由 GraalVM native-image 编译，**不需要安装 Java**。

## 当前版本

- Version: `0.1.5`
- darwin-arm64 (macOS Apple Silicon): [`figma-cli-darwin-arm64`](https://github.com/HelloVass/hellovass-figma-cli/releases/download/v0.1.5/figma-cli-darwin-arm64)（SHA-256: `3a6986c384dbd0566ca06404c80d39aff117cf314e0d9b636dd98552d2716206`）
- linux-x64: [`figma-cli-linux-x64`](https://github.com/HelloVass/hellovass-figma-cli/releases/download/v0.1.5/figma-cli-linux-x64)（SHA-256: `79565b7913856fc0689fa3bfb4ad3591e33d9779f711efc5a2beb622ea142c9c`）

## 安装

下载对应平台的二进制并校验（以 macOS Apple Silicon 为例）：

```bash
curl -fLO https://github.com/HelloVass/hellovass-figma-cli/releases/download/v0.1.5/figma-cli-darwin-arm64
curl -fLO https://github.com/HelloVass/hellovass-figma-cli/releases/download/v0.1.5/figma-cli-darwin-arm64.sha256
shasum -a 256 -c figma-cli-darwin-arm64.sha256
chmod +x figma-cli-darwin-arm64
```

放入 PATH 即可（可选）：

```bash
mkdir -p ~/.local/bin && mv figma-cli-darwin-arm64 ~/.local/bin/figma-cli
```

首次运行 macOS 可能提示 Gatekeeper 拦截（未公证二进制），执行：

```bash
xattr -d com.apple.quarantine ~/.local/bin/figma-cli
```

token 只从环境变量 `FIGMA_TOKEN` 读取，不接受命令行参数：

```bash
export FIGMA_TOKEN="figd_xxx"
```

## 快速开始

```bash
# 顶层与子命令 help 是参数、默认值和输出契约的唯一事实源
figma-cli --help

# 访问 Figma 前先检查本地运行环境；不访问网络，但会检查 FIGMA_TOKEN 是否已设置
figma-cli doctor

# 一次导出 raw、ViewTree IR、截图、截图元数据、资源 manifest 与资源目录
figma-cli pull "https://www.figma.com/design/<file-key>/<file-name>?node-id=<node-id>" --output <case-dir>
```

## 命令

| 命令 | 用途 |
|------|------|
| `tree` | 获取目标节点的 ViewTree IR。 |
| `raw` | 获取 Figma nodes endpoint 原始 JSON。 |
| `frames` | 列出文件内 page 与顶层 frame/component。 |
| `annotations` | 原样保留 REST 原生注释，匹配画布旁注释，并生成逐目标覆盖报告。 |
| `screenshot` | 将目标节点截图写入磁盘，并把含输出路径的元数据 JSON 写到 stdout。 |
| `assets` | 从已落盘的 ViewTree IR 收集并下载资源。 |
| `pull` | 一次生成 raw、IR、截图、截图元数据、资源 manifest 与资源目录。 |
| `doctor` | 在访问 Figma 前检查本地运行环境。 |
| `skills` | 列出、查找、定位、添加、替换或移除 CLI 包内的可信 Skills。 |

调用任一命令前先读取该命令的 `--help`；命令名、参数、默认值与输出契约以 help 为准。

## 边界

`figma-cli` 不修改 Figma 文件，也不生成 Compose / SwiftUI / Flutter / React Native 代码。它输出平台无关的
ViewTree IR 与证据，平台实现由 agent 或下游工程完成。源码与开发流程在私有仓维护，本仓只放分发产物与说明。
