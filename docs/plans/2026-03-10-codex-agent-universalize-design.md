# codex-agent Universalization Design

**Goal:** Make the codex-agent repo broadly reusable by removing personal/instance-specific wording (e.g., “个人称呼示例”等称呼) and avoiding hardcoded chat targets/agent ids in repo code/docs.

**Scope (Option A / minimal-change):**
- Documentation/text only: replace personalized terms with neutral wording.
- Repo code: remove *personal defaults* (hardcoded chat_id / agent name) and return to env-driven placeholders.
- Keep behavior/mechanism otherwise unchanged.

**Non-goals:**
- Do not redesign the hook architecture (tmux + notify + monitor stays).
- Do not add new config formats or new runtime dependencies.
- Do not publish/sync artifacts; only adjust this project repo.

## What to change

### 1) Replace personalized language

**Rule:**
- Replace personal names (e.g., “个人称呼示例”) with neutral wording (“用户/你/请求方”等)，必要时用“用户/老板”区分角色。
- Keep meaning, avoid changing semantics.

**Target files (initial pass):**
- `SKILL.md`
- `workflows/*.md`
- `knowledge/*.md`
- `README*.md`, `INSTALL.md`

### 2) Remove personal defaults from hooks

**Rule:** hooks must be configurable via environment variables; repo defaults should be placeholders.

- `hooks/on_complete.py`
  - `CODEX_AGENT_CHAT_ID` default: `YOUR_CHAT_ID`
  - `CODEX_AGENT_NAME` default: `main`
- `hooks/pane_monitor.sh`
  - same defaults (`YOUR_CHAT_ID`, `main`)

(Keep any non-personal robustness improvements only if they don't change intended workflow; otherwise revert.)

## Success criteria

- Repo 内不再出现个人称呼（示例：`rg -n "(个人称呼关键词)" .` 为 0）。
- hooks contain no hardcoded real chat ids.
- `python3 -m py_compile hooks/on_complete.py` passes.
- `bash -n hooks/pane_monitor.sh hooks/start_codex.sh hooks/stop_codex.sh` passes.

## Rollback

- Revert commit or restore from the git repo history.
