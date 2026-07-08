# Security & privacy

The canonical, always-current security & privacy page for BaseThread and its MCP server is:

## → https://basethread.ai/security

It covers, in reviewer-precise detail:

- **What the MCP server can access** , it acts as you; reads are scoped to your role and workspace
  membership; writes are additive (activity, decisions, tasks, documents).
- **Authentication** , OAuth 2.0 + PKCE, scoped tokens (1h access / 90d refresh, stored hashed),
  revocable anytime; macOS Keychain + auth-free local bridge.
- **Data handling** , encrypted in transit and at rest, never used to train models, workspace-scoped.
- **Permissions & isolation** , server-side RBAC, additive/rate-limited/logged writes, destructive
  actions require explicit confirmation, per-workspace row-level security, no cross-tenant read path.

## Reporting a security issue

Email **hello@basethread.ai**. We aim to acknowledge within two business days.
