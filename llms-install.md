# Install the BaseThread MCP server

BaseThread is one shared context your whole team's AI tools read and write over MCP. Every tool
(Claude Code, Cursor, ChatGPT, Windsurf, Copilot) reads the relevant slice of a shared,
company-structured context graph at the start of a session and writes activity, decisions, and
tasks back as work happens, so no one re-explains the project and answers stop contradicting.

This file lets an agent configure the server with no back-and-forth.

## Prerequisites

- An MCP client that supports **remote / streamable-HTTP** servers with OAuth (Claude Code, Cursor,
  ChatGPT, Windsurf, Copilot, and most current clients do).
- A **BaseThread account** (free tier, no credit card): https://app.basethread.ai
- No local install is required for the remote connection. Sign-in is a one-time browser OAuth approval.

## Add the server (remote, recommended)

The server is hosted at `https://mcp.basethread.ai/mcp` (streamable-HTTP, JSON-RPC 2.0). Authentication
is OAuth 2.0 with PKCE: on first connect your client opens a BaseThread sign-in page, you approve once,
and the client stores a scoped token. You can revoke it anytime from your account.

### Claude Code

```bash
claude mcp add --transport http basethread https://mcp.basethread.ai/mcp
```

Then run any BaseThread command; Claude Code will prompt the one-time OAuth sign-in in your browser.

### Cursor, Windsurf, and other JSON-config clients

Add this to the client's MCP config (e.g. `mcp.json` / the client's MCP settings):

```json
{
  "mcpServers": {
    "basethread": {
      "type": "streamable-http",
      "url": "https://mcp.basethread.ai/mcp"
    }
  }
}
```

On first use the client opens the BaseThread OAuth sign-in. Approve once.

### ChatGPT / web clients

Add a new connector with the URL `https://mcp.basethread.ai/mcp` and complete the sign-in when prompted.

## Verify the connection

Ask the tool to call **`bt_workspace`** (or **`bt_context`** with a question about your work). You should
see your workspace, your role, and the current context. If you see an authorization prompt, complete the
one-time sign-in and retry.

## Tools the server exposes

All tools are prefixed `bt_`. Reads are scoped to your role and workspace; writes are additive and
rate-limited; destructive operations require explicit confirmation (see SECURITY.md).

**Read / recall**
- `bt_workspace` , the workspace, your containers, your role, and behavior directives (call first).
- `bt_context` , the most relevant slice of context for a question (call this proactively).
- `bt_search` , find documents by topic. `bt_read` , fetch one document. `bt_recent` , catch up on changes.
- `bt_list` , discover structure. `bt_scope_tree` , the tree under the current area.
- `bt_decisions` , the settled-decisions ledger. `bt_activity` , the work-events ledger. `bt_tasks` , tasks.
- `bt_members` , who is on a team/project and their role. `bt_person` , one person's profile.
- `bt_meetings` , 1:1 / meeting notes. `bt_conflicts` , open conflicts on decisions.

**Write / record**
- `bt_write_file` , create, update, or append a document.
- `bt_log_decision` / `bt_update_decision` / `bt_delete_decision` , the decisions ledger.
- `bt_log_activity` / `bt_update_activity` / `bt_delete_activity` , the activity ledger.
- `bt_log_meeting` , record a 1:1 / meeting note.
- `bt_create_task` / `bt_update_task` / `bt_accept_task` / `bt_decline_task` / `bt_complete_task` /
  `bt_check_task_item` / `bt_delete_task` , tasks.
- `bt_resolve_conflict` , resolve a decision conflict.

**Scope / structure**
- `bt_set_scope` , set the working area for the session (reads + writes default to it).
- `bt_create_container` / `bt_rename_container` / `bt_move_container` / `bt_delete_container` , structure.
- `bt_move_file` / `bt_delete_file` / `bt_lock_file` / `bt_unlock_file` , file management.
- `bt_seed_progress` , progress of an initial context seed.

## Safety

No destructive action runs without user confirmation. Reads never exceed what you can see in the web app,
writes are additive and logged, and one team's context is isolated from another's. Full details:
[SECURITY.md](./SECURITY.md).

## Local (Mac desktop) alternative

If you run the BaseThread macOS app, it also exposes the same tools over a local stdio bridge to the
desktop daemon (no network round-trip). That path is for the installed desktop app; for registry / fresh
installs use the remote connection above.
