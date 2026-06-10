# 硕士毕业设计资源总仓库（graduation-design）

本仓库是**毕业设计资源聚合仓库**，本身不直接存放内容，而是通过 **git submodule** 将多个子仓库的不同分支挂载为不同文件夹，方便同时引用「开题」「中期」以及配套「学术论文」的所有材料。

> 研究课题：**移位异或纠删码的高效实现研究**（学生：孙铎，导师：王强 副教授，哈尔滨工业大学（深圳）计算机科学与技术学院）

## 目录与子模块映射

| 路径 | 子仓库 | 分支 | 用途 |
| --- | --- | --- | --- |
| `proposal/thesis` | `hitszthesis` | `proposal` | 开题报告（hitszthesis 文档类） |
| `proposal/slides` | `hitszbeamer` | `proposal` | 开题答辩幻灯片 |
| `midterm/thesis`  | `hitszthesis` | `midterm`  | 中期报告（hithesisart 文档类） |
| `midterm/slides`  | `hitszbeamer` | `midterm`  | 中期答辩幻灯片 |
| `paper/tsx-overleaf` | `tsx-overleaf` | `main`   | 学术论文（IPDPS 2026 投稿） |
| `paper/tsx-slides`   | `tsx-slides`   | `master` | 学术论文汇报幻灯片 |

注意：同一个子仓库（如 `hitszthesis`）以**不同分支**被挂载到 `proposal/thesis` 与 `midterm/thesis` 两个不同路径，互不影响。

## 首次克隆

```bash
git clone <本仓库地址> graduation-design
cd graduation-design
git submodule update --init --recursive
```

或一步到位：

```bash
git clone --recurse-submodules <本仓库地址>
```

## 拉取各子模块的最新内容

```bash
# 让每个子模块在其配置的分支（见上表）上拉取远端最新提交
git submodule update --remote --merge
```

## 在子模块中工作

子模块默认可能处于 detached HEAD。进入子模块后先切到目标分支再提交：

```bash
cd midterm/thesis
git checkout midterm          # 切到工作分支
# ... 编辑、编译 ...
git add -A && git commit -m "..."
git push                       # 推送到子仓库的 midterm 分支
```

回到父仓库后，记录子模块的新指针：

```bash
cd ../..                       # 回到 graduation-design
git add midterm/thesis
git commit -m "bump midterm/thesis submodule"
```

## 编译速查

| 目标 | 命令 | 引擎 | 输出 |
| --- | --- | --- | --- |
| 中期报告 `midterm/thesis` | `mkdir -p build/front build/body && latexmk report.tex` | xelatex（latexmkrc 内置） | `build/report.pdf` |
| 中期幻灯片 `midterm/slides` | `xelatex -interaction=nonstopmode main.tex`（×3 + bibtex） | xelatex | `main.pdf` |
| 开题报告 `proposal/thesis` | `latexmk main.tex` | xelatex | `main.pdf` |
| 论文 `paper/tsx-overleaf` | `latexmk main.tex` 或 `pdflatex` | 视模板 | `main.pdf` |

> Linux/CI 环境的中文字体请使用 `fontset=fandol`（开源）。Windows/Mac 本地可改回 `windows`/`mac`。

## 将父仓库推送到 GitHub（可选）

本仓库目前为本地仓库。若要托管到 GitHub：

```bash
git remote add origin git@github.com:d-sun-cs/<新仓库名>.git
git push -u origin main
```

详细的协作与 agent 交接约定见 [`AGENTS.md`](AGENTS.md)。
