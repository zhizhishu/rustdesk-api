# Local AGENTS Instructions (rustdesk-api fork)

## Priority
- This file overrides global/default agent behavior for this repository.
- Keep changes minimal and traceable. Prefer additive changes over rewrites.

## Branch Workflow
- `upstream` = `https://github.com/lejianwen/rustdesk-api.git`
- `origin` = your fork repository
- `master`
  - only used to sync upstream
  - do not place fork-only features here
- `custom`
  - long-term fork customization branch
  - production-ready fork features land here after verification
- `future` (optional but recommended)
  - pre-integration branch for upstream merge digestion and regression checks
- `feature/*`
  - short-lived branch for one feature or one bugfix
  - merge back to `future` (or `custom` if `future` is not used)

## Task Log Protocol (Mandatory)
- Before any work, check `TASK_LOG.md` and align context.
- After planning, immediately add the first concrete target:
  - `- [ ] **目标:** <描述> (创建于: YYYY-MM-DD HH:MM:SS)`
- After finishing one target, immediately mark complete:
  - `- [x] ~~**目标:** <描述>~~ (创建于: ... | **完成于: ...**)`
- Every completed target should include:
  - changed files
  - whether it is fork-only or upstream-friendly
  - upstream sync/merge risk

## MCP Usage Policy
- Prefer local analysis/edit/test tools first.
- Use external MCP/services only when needed.
- Keep each external call scoped and explain why.
- Add call summary in response using:
  - `【MCP调用简报】 服务: ... | 触发: ... | 参数: ... | 结果: ... | 状态: ...`

## Change Strategy
- Follow KISS/YAGNI/DRY/SOLID.
- Avoid broad refactors without explicit request.
- Preserve existing behavior unless change scope explicitly requires behavior change.
- For cross-file updates, define interfaces first, then implement.

## Validation
- Prefer fast local verification first.
- Typical checks:
  - `go test ./...` (or focused package tests)
  - basic run smoke test (`go run cmd/apimain.go` when needed)
  - critical flow checks for auth / address book / web admin pages

## Safety
- Do not touch secrets, runtime credentials, or deployment env files unless requested.
- Do not rewrite history or force-push shared branches unless requested.
