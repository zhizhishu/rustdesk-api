# rustdesk-api 分支协作规范（Fork 二创版）

本文档用于约束本仓库的同步、开发、验证与发布流程，确保二创能力长期可维护、可追溯。

## 分支职责

- `master`
  - 仅用于同步 `upstream/master`
  - 不承载 fork 私有功能开发
- `custom`
  - 稳定二创分支
  - 部署默认使用（如你选择二创版本发布）
- `future`（可选）
  - 预集成分支，用于冲突消化与回归验证
  - 验证通过后再合并到 `custom`
- `feature/*`
  - 单功能/单修复短分支
  - 建议从 `future` 拉出并回合到 `future`

## 同步策略

1. 同步上游：`upstream/master -> master`
2. 预集成：`master -> future`（可选）
3. 发布集成：`future -> custom`（或直接 `master -> custom`，不建议长期使用）
4. 热修复：若在 `custom` 紧急修复，需回灌到 `future`

## 任务日志规则（强制）

每轮开发必须更新 `TASK_LOG.md`：

- 创建任务：
  - `- [ ] **目标:** <描述> (创建于: YYYY-MM-DD HH:MM:SS)`
- 完成任务：
  - `- [x] ~~**目标:** <描述>~~ (创建于: ... | **完成于: ...**)`

每个完成项必须记录：

- 改动文件
- 是否 fork-only
- 上游同步风险点

## MCP 使用规则

- 优先本地工具（读代码、改代码、跑测试）。
- 外部 MCP 调用保持最小范围并可解释。
- 响应尾部追加统一简报：
  - `【MCP调用简报】 服务: ... | 触发: ... | 参数: ... | 结果: ... | 状态: ...`

## 与 rustdesk-server 的配合建议

- 可先使用 `lejianwen/rustdesk-server` 镜像，不强制 fork。
- 若需要改 server 行为、固定发布节奏、或避免上游漂移，建议再 fork `rustdesk-server`。
- 建议生产环境固定镜像版本（tag/digest），避免仅使用漂移 `latest`。

## 本地常用命令

```bash
# 1) 配置上游（在 origin 已是你的 fork 前提下）
git remote add upstream https://github.com/lejianwen/rustdesk-api.git
git fetch upstream

# 2) 同步 master
git checkout master
git merge --ff-only upstream/master || git merge --no-edit upstream/master
git push origin master

# 3) 创建 custom/future（首次）
git checkout -b custom
git push -u origin custom
git checkout -b future
git push -u origin future

# 4) 功能开发
git checkout future
git checkout -b feature/your-feature
```
