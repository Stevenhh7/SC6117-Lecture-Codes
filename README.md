# SC6117 Lecture Codes

NTU **SC6117 — Data Science Project in Practice** 课程的学习与练习仓库，用于记录课堂代码、Jupyter Notebook 和 Python 环境配置。

项目使用 **Python 3.12**，通过 **[uv](https://docs.astral.sh/uv/)** 管理 Python 版本、虚拟环境和依赖，并使用 **JupyterLab** 运行 Notebook。

## 当前内容

目前包含 `sc6117-test` 项目，用于验证 Python 与 JupyterLab 环境。

```text
SC6117-Lecture-Codes/
├── README.md
└── sc6117-test/
    ├── .python-version    # 项目使用的 Python 版本
    ├── pyproject.toml     # 项目配置与依赖
    ├── uv.lock            # 锁定依赖版本
    ├── main.py            # Python 运行验证
    ├── Test.ipynb         # Notebook 与解释器验证
    └── Untitled.ipynb     # 空白练习 Notebook
```

本地的 `.venv/` 由 uv 创建，用来保存该项目独立的 Python 环境。

## 快速开始

以下命令以 Windows PowerShell 为例。

### 1. 安装 uv

如果已经安装 uv，可直接检查版本并继续下一步。

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

安装完成后，重新打开终端并确认：

```powershell
uv --version
```

其他平台的安装方法见 [uv 官方安装文档](https://docs.astral.sh/uv/getting-started/installation/)。

### 2. 获取代码并安装环境

```powershell
git clone https://github.com/Stevenhh7/SC6117-Lecture-Codes.git
cd .\SC6117-Lecture-Codes\sc6117-test

uv python install 3.12
uv sync --locked
```

如果已经下载或克隆本仓库，直接进入 `sc6117-test` 目录后执行最后两条命令即可。

`uv sync --locked` 会根据 `uv.lock` 创建或同步 `.venv`，安装项目依赖及默认的开发依赖，包括 JupyterLab、Jupyter 和 Notebook。

### 3. 验证 Python

```powershell
uv run python --version
uv run python main.py
```

Python 版本应为 `3.12.x`，运行示例脚本后应看到 `Hello, Jupyter!` 和 Python 版本信息。

### 4. 启动 JupyterLab

在 `sc6117-test` 目录中运行：

```powershell
uv run jupyter lab
```

打开终端输出的本地访问地址，然后打开 [Test.ipynb](sc6117-test/Test.ipynb) 或新建 Notebook。

使用 `uv run` 时，无需手动激活 `.venv`。在启动服务的终端按 `Ctrl+C`，并根据提示确认，即可停止 JupyterLab。

## 选择与验证 Kernel

在 JupyterLab 的 **Kernel → Change Kernel** 中选择 **Python 3 (ipykernel)**。

Kernel 的显示名称本身不代表具体的 Python 版本。可以运行以下单元格，确认 Notebook 使用了项目的解释器：

```python
import sys

print(sys.version)
print(sys.executable)
```

预期结果：

- Python 版本为 `3.12.x`。
- Windows 下解释器路径以 `sc6117-test\.venv\Scripts\python.exe` 结尾。

如果需要为项目设置容易识别的 Kernel 名称，在 `sc6117-test` 目录执行：

```powershell
uv run python -m ipykernel install --sys-prefix --name sc6117-test --display-name "Python 3.12 (sc6117-test)"
```

随后重新启动或刷新 JupyterLab，选择 **Python 3.12 (sc6117-test)**。该 Kernel 注册在当前项目的 `.venv` 中；重建 `.venv` 后需要重新注册。

## 日常使用

以下命令均在 `sc6117-test` 目录中执行。

| 操作 | 命令 |
| --- | --- |
| 同步已锁定的环境 | `uv sync --locked` |
| 运行 Python 脚本 | `uv run python main.py` |
| 启动 JupyterLab | `uv run jupyter lab` |
| 查看已安装的 Python | `uv python list --only-installed` |
| 查看项目依赖树 | `uv tree` |

新增依赖时使用 `uv add <包名>`，开发工具使用 `uv add --dev <包名>`。更新依赖后，将 `pyproject.toml` 与 `uv.lock` 一起提交，以便在其他电脑上复现环境。

## 常见问题

### `uv python list` 看不到安装在自定义目录中的 Python

使用 `uv python dir` 检查 uv 当前查找的安装目录。如果 Python 安装在自定义位置，需要设置 `UV_PYTHON_INSTALL_DIR` 指向该目录。具体方法见 [uv 存储目录说明](https://docs.astral.sh/uv/reference/storage/#python-versions)。

### Notebook 使用了其他环境

先通过 `sys.executable` 检查解释器路径，再按上面的步骤注册并选择项目 Kernel。始终从项目目录使用 `uv run jupyter lab` 启动。

## 参考资料

- [uv 项目管理指南](https://docs.astral.sh/uv/guides/projects/)
- [uv 与 Jupyter 集成](https://docs.astral.sh/uv/guides/integration/jupyter/)
- [JupyterLab 文档](https://jupyterlab.readthedocs.io/en/stable/)
