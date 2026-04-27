# MyClaw Hermes 分支与同步规范

## 背景与目标

本仓库用于承载 MyClaw 对 Hermes 的长期定制。为了后续持续同步官方 Hermes 更新，同时避免把定制开发污染到主分支，分支使用和同步流程必须固定。

## 分支职责

- `main`
  - 只作为官方 Hermes 的同步基线。
  - 不承载 MyClaw 日常定制开发。
- `feat-myclaw`
  - 作为 MyClaw 的长期开发分支。
  - 所有 Hermes 定制改造默认都提交到这个分支。
- `feat/*`
  - 可选的短期功能分支。
  - 如果后续改动变大，可从 `feat-myclaw` 拉出，完成后再合回 `feat-myclaw`。

## 日常开发规则

- 默认在 `feat-myclaw` 上开发。
- 不直接向 `main` 提交 MyClaw 定制。
- 本地 `main` 不要求始终最新。
- 只有在准备同步官方更新时，才主动更新本地 `main`。

## 官方更新同步流程

### 1. 先同步 fork 的 `main`

如果远端 fork 的 `main` 已经同步了官方更新，本地执行：

```bash
git checkout main
git fetch origin main
git merge --ff-only origin/main
```

如果需要直接从官方仓库同步，再执行：

```bash
git checkout main
git fetch upstream main
git merge upstream/main
git push origin main
```

### 2. 再把 `main` 合入 `feat-myclaw`

```bash
git checkout feat-myclaw
git merge main
```

如果有冲突，优先保留：

- 官方修复和基础设施更新
- MyClaw 仍然需要的定制逻辑

冲突解决后：

```bash
git add .
git commit
git push origin feat-myclaw
```

## 推荐工作方式

- 能在外层做的改造，不直接改 Hermes 核心。
- 定制代码尽量集中，避免大面积分散修改。
- 每次同步官方更新时，小步合并，不要积压太久。

## 禁止事项

- 不在 `main` 上做 MyClaw 日常开发。
- 不对 `main` 执行 `force push`。
- 不随意改写 `main` 历史。
- 不把 AI 工作台主项目的前端、文档、主后端代码混入本仓库。

## 当前约定总结

一句话原则：

`main` 负责跟官方，`feat-myclaw` 负责做定制；官方更新时永远走 `main -> feat-myclaw`。
