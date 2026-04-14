# Resume

蔡礼珂（Like CAI）的简历源文件与自动构建仓库。

## 最新版本下载

最新通用版简历始终可从下方链接获取：

- 中文版：https://github.com/cailikelog/resume/releases/latest/download/LikeCAI-Resume-CN.pdf
- 英文版：https://github.com/cailikelog/resume/releases/latest/download/LikeCAI-Resume-EN.pdf

查看所有历史版本：https://github.com/cailikelog/resume/releases

## 自动构建说明

本仓库使用 GitHub Actions 自动编译 LaTeX 简历。每次推送符合规范的 tag 时，Actions 会编译对应的 tex 源文件并发布为 Release。

### Tag 命名规范

本仓库使用 tag 驱动的发布流程，tag 命名决定发布行为：

| Tag 格式 | 用途 | 示例 | 是否设为 latest |
|---|---|---|---|
| `v{yyyy.mm.dd}` | 通用版发布，面向所有场合 | `v2026.04.14` | 是 |
| `company-{name}-{yyyy.mm}` | 定制版发布，针对特定公司或岗位 | `company-bytedance-2026.04` | 否 |

说明：

- 通用版 tag 推送后，对应 Release 会自动成为 `latest`，`releases/latest/download/` 的下载链接会指向它。
- 定制版 tag 推送后，Release 会正常创建并可通过 tag 名访问，但不会覆盖 `latest`，保证稳定发送出去的通用版链接始终有效。
- 公司名使用英文小写 + 短横线，不使用中文、空格、点号等特殊字符。
- 同一家公司多次投递时，通过日期或岗位后缀区分，例如 `company-bytedance-2026.04-robotics`。

几个提示：

如果发现 tag 打错了想删掉重来，本地删 git tag -d v2026.04.14，远程删 git push origin --delete v2026.04.14，然后重新打。但注意如果 Actions 已经创建了 Release，还要去 GitHub 网页手动删掉对应的 Release（删 tag 不会自动删 Release）。
以后每次打 tag 前最好确认 main 分支的源文件已经是你想要发布的状态，因为 Actions 编译的是 tag 指向的那个 commit 的内容。
第一次正式发布跑通后，建议立刻把 releases/latest/download/ 链接填回到 tex 源文件里（作为"在线最新版"的超链接），然后再打一个新 tag v2026.04.14-1 或者 v2026.04.15 重新发布一次。这样 pdf 里的链接就形成了自引用的闭环。

### 发布流程示例

发布一个通用版：

    git add 2026Sp-main-cn.tex
    git commit -m "更新项目经历"
    git push
    git tag v2026.04.14
    git push origin v2026.04.14

为某公司定制并发布：

    # 修改简历内容
    git commit -am "为 ByteDance 定制"
    git tag company-bytedance-2026.04
    git push origin company-bytedance-2026.04

## 源文件说明

| 文件 | 用途 | 是否自动编译 |
|---|---|---|
| `2026Sp-main-cn.tex` | 2026 春季主版本，中文 | 是 |
| `2025Sp-main-cn.tex` | 历史版本，归档用 | 否 |
| `2025Sp-main-en.tex` | 历史版本，归档用 | 否 |

自动编译的 PDF 输出文件名：

- `2026Sp-main-cn.tex` → `LikeCAI-Resume-CN.pdf`
- `2026Sp-main-en.tex` → `LikeCAI-Resume-EN.pdf`

## 编译环境

- 编译器：XeLaTeX
- 依赖宏包：ctex、xeCJK、fontspec 等
- 字体：Noto Serif CJK SC、Noto Sans CJK SC
- 构建环境：GitHub Actions + xu-cheng/latex-action
