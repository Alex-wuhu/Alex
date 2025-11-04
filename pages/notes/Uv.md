---
title: uv Cheat Sheet
date: 2025-11-04
type: note
tags:
  - CLI
---

uv = 新一代超高速 Python 包管理 + 构建 + CLI 工具（Rust 实现）
替代：pip、virtualenv、pipx、poetry install、pip install -e、pip-run 等。

[[toc]]

## 安装 uv

**curl:**

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Homebrew (macOS):**

```bash
brew install uv
```

**确认版本：**

```bash
uv --version
```

## 包管理

### 包安装 / 卸载 / 更新

**添加并安装包：**

```bash
uv add <package>
```

**卸载并移除包：**

```bash
uv remove <package>
```

**同步安装环境依赖：**

```bash
uv sync
```

**安装本地项目（开发模式）：**

```bash
uv pip install -e .
```

### 项目依赖管理

**安装 pyproject.toml 中所有依赖：**

```bash
uv pip install .
```

**安装锁定依赖：**

```bash
uv pip sync
```

**生成锁文件：**

```bash
uv lock
```

> [!NOTE]
> 修改依赖后记得：
> `uv pip install .`

### 查看已安装包

**列出已安装包:**

```bash
uv pip list
```

**查看项目依赖树：**

```bash
uv pip tree
```

## 运行 Python 程序

**运行脚本：**

```bash
uv run script.py
```

**带依赖临时执行：**

```bash
uv run --with requests script.py
```

**运行模块：**

```bash
uv run -m <module>
```

## 管理 CLI 工具（替代 pipx）

**安装 CLI 工具:**

```bash
uv tool install <package>
```

**安装当前项目：**

```bash
uv tool install .
```

之后可全局使用：

```bash
polymarket-agents --help
```

**列出已安装工具:**

```bash
uv tool list
```

**升级工具:**

```bash
uv tool upgrade <tool>
```

**卸载工具:**

```bash
uv tool remove <tool>
```

## 使用 uvx（临时运行 CLI：类似 npx）

**一次性运行：**

```bash
uvx <package> <args>
```

**例如：**

```bash
uvx ruff check .
```

> [!TIP]
> 无需安装！

**运行 HTTP 请求：**

```bash
uvx httpx https://example.com
```

**运行 python-repl：**

```bash
uvx ipython
```

## 虚拟环境管理

**创建 venv：**

```bash
uv venv
```

**指定 Python 版本：**

```bash
uv venv --python 3.11
```

**激活 venv (bash/zsh):**

```bash
source .venv/bin/activate
```

**fish:**

```bash
source .venv/bin/activate.fish
```

**移除 venv：**

```bash
rm -rf .venv
```

## 项目初始化与构建

### 快速项目初始化（Poetry 风格）

```bash
uv init
```

会创建：

- `pyproject.toml`
- `.venv`
- 基本结构

### 项目构建

**构建 wheel + sdist：**

```bash
uv build
```

输出存放在 `dist/`。

## 发布包

```bash
uv publish
```

支持：

- PyPI
- 自建 index

## 清理缓存

**清理缓存:**

```bash
uv cache clean
```

**查看缓存占用：**

```bash
uv cache list
```

## 全局设置 Python 版本

**示例：使用 3.11**

```bash
uv python install 3.11
```

**查看已安装版本：**

```bash
uv python list
```

## 推荐工作流

### 初始化新项目

> [!TIP]
> 从零开始一个新项目：
>
> ```bash
> # 初始化项目
> uv init
> # 添加依赖
> uv add <package>
> # 以可编辑模式安装项目本身
> uv pip install -e .
> ```
