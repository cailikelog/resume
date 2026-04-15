# Resume

蔡礼珂（Like CAI）的简历源文件与自动构建仓库。

## 最新版本下载

最新通用版简历始终可从下方链接获取：

- 中文版：https://github.com/cailikelog/resume/releases/latest/download/LikeCAI-Resume-CN.pdf
- 英文版：https://github.com/cailikelog/resume/releases/latest/download/LikeCAI-Resume-EN.pdf

查看所有历史版本：https://github.com/cailikelog/resume/releases

## 自动构建说明

本仓库使用 GitHub Actions 自动编译 LaTeX 简历。每次推送符合规范的 tag 时，Actions 会编译对应的 tex 源文件并发布为 Release。

### 文件命名规范

**源文件**：`{year}{season}-{note}-{lang}.tex`

| 字段 | 说明 | 示例 |
|---|---|---|
| `{year}{season}` | 年份 + 招聘季（`Sp` 春招 / `Fall` 秋招） | `2026Sp`、`2027Fall` |
| `{note}` | 版本说明，通用版固定为 `main` | `main`、`3D_printing`、`robotics` |
| `{lang}` | 语言（`cn` / `en`） | `cn`、`en` |

**PDF 输出**：从 tex 文件名自动推导

```
{year}{season}-main-cn.tex    →  LikeCAI-Resume-CN.pdf
{year}{season}-main-en.tex    →  LikeCAI-Resume-EN.pdf
{year}{season}-{note}-cn.tex  →  LikeCAI-Resume-{note}-CN.pdf
```

### Tag 命名规范

本仓库使用 tag 驱动的发布流程，tag 命名决定发布行为：

| Tag 格式 | 用途 | 示例 | 是否设为 latest |
|---|---|---|---|
| `main-v{yyyy.mm.dd}` | 通用版发布，面向所有场合 | `main-v2026.04.14` | 是 |
| `special-v{yyyy.mm.dd}` | 定制版发布，针对特定岗位类型 | `special-v2026.05.04` | 否 |

说明：

- 通用版 tag 推送后，对应 Release 会自动成为 `latest`，`releases/latest/download/` 的下载链接会指向它。
- 定制版 tag 推送后，Release 会正常创建并可通过 tag 名访问，但不会覆盖 `latest`，保证已发出的通用版链接始终有效。
- 定制版是针对**岗位类型**而非特定公司，同一份定制简历可投多家公司。同一岗位方向多次修订时，通过日期区分。
- `main-*` tag 触发时只编译 `*-main-*.tex`；`special-*` tag 触发时编译仓库中所有非 main 的 tex 文件。

几个提示：

如果发现 tag 打错了想删掉重来，本地删 `git tag -d main-20260414`，远程删 `git push origin --delete main-20260414`，然后重新打。但注意如果 Actions 已经创建了 Release，还要去 GitHub 网页手动删掉对应的 Release（删 tag 不会自动删 Release）。

每次打 tag 前最好确认 main 分支的源文件已经是你想要发布的状态，因为 Actions 编译的是 tag 指向的那个 commit 的内容。

### 发布流程示例

发布通用版：

    git add 2026Sp-main-cn.tex
    git commit -m "更新项目经历"
    git push
    git tag main-v2026.04.14
    git push origin main-v2026.04.14

发布定制版（将 `2026Sp-3D_printing-cn.tex` 放入仓库后）：

    git commit -am "3D 打印岗位定制版"
    git tag special-v2026.05.04
    git push origin special-v2026.05.04

## 编译环境

- 编译器：XeLaTeX
- 依赖宏包：ctex、xeCJK、fontspec 等
- 字体：Noto Serif CJK SC、Noto Sans CJK SC
- 构建环境：GitHub Actions + xu-cheng/latex-action
