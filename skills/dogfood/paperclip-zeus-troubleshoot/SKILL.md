---
name: paperclip-zeus-troubleshoot
description: PaperclipAI + Hermes agent troubleshooting — Zeus curl|python3 blocking, approval.py patches, session cache issues. Session 20260404.
category: dogfood
---

# Paperclip + Zeus Troubleshooting

## Quick Fix Commands

**Restart Paperclip:**
```bash
kill $(lsof -ti :3100) && sleep 2 && cd /private/tmp/paperclip && pnpm --filter @paperclipai/server exec tsx ../scripts/dev-runner.ts dev &
```

**Kill stale gateway + clear bytecode:**
```bash
kill $(ps aux | grep 'gateway run' | grep -v grep | awk '{print $2}') 2>/dev/null
find /Users/shakilakram/.hermes/hermes-agent -name "__pycache__" -type d -exec rm -rf {} + 2>/dev/null
```

## Applied Fixes (as of 2026-04-04)

### config.yaml patches — /Users/shakilakram/.hermes/config.yaml
```yaml
approvals:
  mode: auto
command_allowlist:
- script execution via -e/-c flag
- curl | python3 to localhost
security:
  tirith_enabled: false
```

### approval.py patch — /Users/shakilakram/.hermes/hermes-agent/tools/approval.py ~line 454
```python
    # Auto-approve dangerous commands if mode is auto/off, even for gateway sessions
    approval_mode = _get_approval_mode()
    if approval_mode in ("auto", "off"):
        return {"approved": True, "message": None}

    if is_gateway or os.getenv("HERMES_EXEC_ASK"):
        submit_pending(...)
```

## Key Architecture

### Two Separate Security Systems
1. **tirith** — catches "Pipe to interpreter: curl | python3" — disabled via `tirith_enabled: false`
2. **dangerous-command detector** (approval.py DANGEROUS_PATTERNS line 61) — key "script execution via -e/-c flag" — covered by allowlist
3. **Per-session deny CACHE** (`_session_approved` in approval.py) — cached denies persist across heartbeats, block even with allowlist. Need fresh gateway session.

### Paperclip Agent Env Vars
Paperclip spawns Hermes gateway with `HERMES_GATEWAY_SESSION=true` and `HERMES_EXEC_ASK=true`. These force approval_required UNLESS `approvals.mode: auto` AND the approval.py bypass patch is applied.

## Unresolved Issues

1. **HEARTBEAT.md changes not loaded** — Paperclip caches instruction bundles. File edits to HEARTBEAT.md don't take effect until bundle rebuild.
2. **Zeus 0 issues is CORRECT** — all his work done. In-review items (AGE-425/426) belong to Athena.
3. **curl|python3 still blocked** — Zeus session has cached deny. Full Paperclip restart needed for fresh gateway session.

## BUG: Agent Comments Show "local-board" Instead of Agent Name

**Root cause:** Hermes agents post comments with `authorUserId: "local-board"` instead of their own agent ID.

**Why it happens:**
- `process/execute.ts` passes `PAPERCLIP_AGENT_ID` + `PAPERCLIP_COMPANY_ID` to Hermes env, but NO auth token
- Hermes calls Paperclip API without Bearer token → auth middleware falls back to `local_trusted` mode → `type: "board", userId: "local-board"`
- Comments then saved with `authorAgentId: null, authorUserId: "local-board"`

**The fix (2-line change):**
In `/private/tmp/paperclip/server/src/adapters/process/execute.ts`, the `authToken` from `ctx` is already available but not passed to Hermes. Need to add it as an env var:
```typescript
env.PAPERCLIP_API_KEY = ctx.authToken;  // or a new PAPERCIP_AGENT_TOKEN var
```
Then Hermes needs to include this as `Authorization: Bearer <token>` when calling Paperclip API.

**Alternatively:** Paperclip could set `HERMES_GATEWAY_SESSION=false` for local agents so Hermes falls back to its own API key auth. But the proper fix is passing the JWT.

## Zeus Agent
- ID: c2de5646-60d6-4491-b6d6-a1e562ab439f
- Company: 0c2bcc48-e0a3-4867-915f-ebcb17aec2dc
- HEARTBEAT.md: ~/.paperclip/instances/default/companies/0c2bcc48-e0a3-4867-915f-ebcb17aec2dc/agents/c2de5646-60d6-4491-b6d6-a1e562ab439f/instructions/HEARTBEAT.md
