# Agent 交接说明（AGENTS.md）

本文件用于在不同 AI agent / 协作者之间交接「硕士毕业设计」的工作模式。请在开始任何工作前完整阅读。

## 1. 背景与目标

- 课题：**移位异或纠删码的高效实现研究**。
- 学生：孙铎（学号 24S051046），导师：王强 副教授，哈尔滨工业大学（深圳）计算机科学与技术学院。
- 毕业设计推进阶段：开题 → 中期 → **结题论文初始化（当前）** → 最终答辩。
- 毕业设计内容 = 已产出的一篇学术论文（IPDPS 2026 投稿，FastTT / shift-XOR）+ 后续在其基础上扩展的**新创新点**（中期阶段无需完成新创新点，仅需规划）。

## 2. 仓库与分支总览

本聚合仓库 `graduation-design` 通过 git submodule 组织以下子仓库：

| 子仓库 | 远端 | 已用分支 | 文档类 / 引擎 | 含义 |
| --- | --- | --- | --- | --- |
| hitszthesis | `git@github.com:d-sun-cs/hitszthesis.git` | `proposal`、`midterm`、`final` | proposal/final→hitszthesis；midterm→hithesisart；均 xelatex | 阶段报告与硕士学位论文 |
| hitszbeamer | `git@github.com:d-sun-cs/hitszbeamer.git` | `proposal`、`midterm` | beamer + ctex，xelatex | 答辩幻灯片 |
| tsx-overleaf | `git@github.com:d-sun-cs/tsx-overleaf.git` | `main` | IEEEtran | 学术论文正文 |
| tsx-slides | `git@github.com:d-sun-cs/tsx-slides.git` | `master` | beamer | 学术论文汇报幻灯片 |

挂载路径见根目录 `README.md` 的映射表。

### 分支命名约定
- 毕业设计类仓库（hitszthesis / hitszbeamer）：每个阶段一个分支，已有 `proposal`、`midterm`；结题论文使用 `hitszthesis` 的 `final` 分支，挂载为 `final/thesis`。
- 论文类仓库（tsx-overleaf / tsx-slides）：保持各自原有主分支（`main` / `master`），**不为毕业设计阶段新建分支**。

## 3. 中期分支是怎么来的（重要）

`hitszthesis` 的 `proposal` 分支是**仅适用于开题**的模板（`hitszthesis` 文档类，封面与页眉硬编码为「开题报告」）。中期需要不同格式，因此 `midterm` 分支做了如下整合：

- 引入 [ruicx/HITSZ-Mid-Term-LaTex-Template](https://github.com/ruicx/HITSZ-Mid-Term-LaTex-Template) 的 `hithesisart` 文档类（`hithesisart.cls` / `.cfg` / `hithesis.bst`）。
- 主文件改为 `report.tex`，选项 `type=master, stage=midterm, campus=shenzhen, fontset=fandol`。
- 正文 `body/report_midterm.tex`，封面 `front/coverart.tex`（已填本人信息）。
- 复用 proposal 的 `reference.bib` 与 `figures/`（含 `hitlogo.eps`）。
- 删除了 proposal 专用的 `hitszthesis.*`、`main.tex`、章节与中间产物。

`hitszbeamer` 的 `midterm` 分支基于 `proposal`，仅把封面副标题「开题报告」改为「中期报告」、日期改 `\today`，主题与正文结构作为中期幻灯片起点保留。

> 两个学位仓库的 proposal 与 midterm 文档类不同，**不要**简单地把 proposal 的章节直接 copy 到 midterm；应按中期模板的 section 结构填写内容。

## 4. 标准工作流

### 4.1 获取/同步
```bash
git submodule update --init --recursive      # 首次
git submodule update --remote --merge        # 同步各分支最新
```

### 4.2 在子模块内编辑并提交
```bash
cd <子模块路径>
git checkout <该子模块对应分支>     # 见第 2 节表格，避免 detached HEAD
# 编辑 + 本地编译验证
git add -A && git commit -m "..."
git push
cd -                                # 回父仓库
git add <子模块路径> && git commit -m "bump <子模块> pointer"
```

### 4.3 编译验证（务必本地编译通过再提交）
- 中期报告：`cd midterm/thesis && mkdir -p build/front build/body && latexmk report.tex` → `build/report.pdf`
- 中期幻灯片：`cd midterm/slides && xelatex -interaction=nonstopmode main.tex`（需 3 遍 + 中间 `bibtex main`）→ `main.pdf`
- 结题硕士论文：`cd final/thesis && latexmk main.tex` → `main.pdf`；首次编译会自动生成本地类文件，也可用 `make thesis`。
- 本环境 `latexmk` / `xelatex` 可用；中文用 `fontset=fandol`（开源），不要用 `windows`（缺字体会报 Missing character）。

## 5. Git 身份与权限

- 本环境通过 SSH 以 GitHub 用户 `d-sun-cs` 认证，四个子仓库均有推送权限。
- 无 `gh` CLI、无全局 git 身份。各仓库已设置 per-repo 身份：
  `user.name=d-sun-cs`，`user.email=d-sun-cs@users.noreply.github.com`。新克隆的仓库需重新设置。
- 父仓库 `graduation-design` 已托管在 `git@github.com:d-sun-cs/graduation-design.git`（分支 `main`）。

## 6. 当前状态（交接时）

- **工作根目录 = 父仓库 `graduation-design`**。后续 AI agent 直接以本目录为项目根打开，子仓库内容通过 submodule 访问（`proposal/`、`midterm/`、`final/`、`paper/`）。
- [x] `hitszthesis@midterm` 已创建并推送，可编译。
- [x] `hitszbeamer@midterm` 已创建并推送，可编译。
- [x] `hitszthesis@final` 已建立，初始论文内容通过本地及全新检出编译，详见第 8 节。
- [x] 父仓库 `graduation-design` 使用 submodule 组织 7 个挂载点。
- [x] 中期报告与答辩幻灯片正文已撰写，后续按实际研究进展维护。
- [ ] 论文新创新点（毕业设计扩展点）尚未开展，中期阶段仅需规划。

> 给后续 agent：在父仓库根目录工作时，编辑子模块内文件后，需在**子模块内** commit & push（注意先 `git checkout <分支>` 避免 detached HEAD），再回父仓库 `git add <子模块路径> && git commit` 记录指针，最后 `git push` 父仓库。详见第 4 节。

## 7. 下一步建议

1. 在 `final/thesis` 中依据已有FastTT论文与中期材料逐步完善正文，补充真实实验结果与分析。
2. 核实分类号、答辩日期和成果发表信息；第二个创新点按实际进展更新，格式要求对照上游模板与学院最新规范。
3. 每次改动遵循第 4 节工作流，先本地编译通过再 push，并在父仓库 bump 指针。

## 8. 结题学位论文

- 路径 `final/thesis`，远端仍为 `d-sun-cs/hitszthesis`，工作分支 `final`，从该仓库的 `main` 建立。
- 使用上游 YangLaTeX/hitszthesis v3.4 的正式学位论文类，配置 `type=master,fontset=fandol`。不要沿用开题分支中改写为开题报告的类文件，也不要使用中期的 `hithesisart`。
- 入口 `main.tex`，封面和摘要在 `front/coverinformation.tex`，正文在 `body/chapter01.tex` 至 `chapter05.tex`。
- `latexmkrc` 自动从 `.dtx`/`.ins` 生成本地类文件；生成的 `.cls`/`.cfg`/`.ist` 和 `dtx-style.sty` 不入库，论文输出 `main.pdf` 入库。
- 当前正文用于编译验证，包含已有FastTT工作的整理框架及局部更新研究方案；不得将占位内容误写为已完成实验或最终结论。分类号、具体答辩日期和发表信息待核实，封面年月预填 2027 年 5 月。
- 沿用第 4 节的父子仓库提交顺序：先编译并推送 `hitszthesis@final`，再提交和推送父仓库指针。
