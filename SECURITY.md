# BaseThread MCP , security & permissions

How the BaseThread MCP server handles access, authentication, and your data. Written for anyone vetting
BaseThread for their team.

## What the server can access

The server acts **as you**. Through it, your AI tool can:

- **Read** the slice of your shared context graph you are allowed to see , files, decisions, activity,
  tasks, people, and meeting notes , scoped to your role and your workspace membership.
- **Write** additively: log activity, decisions, and tasks, and create or update documents.

It can **never** see or change something you could not see or change yourself in the web app. Private areas
you are not a member of are invisible, including to your AI. The `author` and `scope` filters on the tools
only narrow within what you can already see; they never widen it.

## Authentication

- The remote endpoint (`https://mcp.basethread.ai/mcp`) authenticates with **OAuth 2.0 + PKCE**. Your AI
  tool receives a **scoped access token**, never your password.
- Access tokens are short-lived (1 hour) and refreshed with a longer-lived refresh token (90 days). Tokens
  are stored **hashed** server-side, never in plaintext.
- One authorization covers every workspace you are a member of; your AI names the workspace per request.
- You can **revoke** a connected tool at any time from your account, which immediately invalidates its tokens.
- On the macOS desktop app, sign-in lives in the **macOS Keychain**. The local bridge your AI tool talks to
  is **auth-free**: your tokens are never sent across the local socket to it.

## Data handling

- **Encrypted in transit and at rest.**
- **Never used to train AI models.** Your content is not used for model training.
- **Scoped to your workspace.** Personal and team workspaces are separate; the server never mixes them.
- Context is stored in BaseThread's managed database. On the desktop app, a local mirror stays on your
  machine, and local deletions go to the system Trash.

## Permissions & blast radius

- **Reads** are bounded by your role (owner / admin / member / viewer at the workspace level; manager /
  member at the container level) and your container membership. Server-side enforcement, not client-side.
- **Writes** are additive (append to the ledgers, create/update documents) and are **rate-limited and
  logged** the same as any write from the web app.
- **Destructive operations require explicit user confirmation.** Deletes and structural changes are not
  performed silently by the AI.

## Tenancy

Every workspace's context is isolated from every other's by server-side row-level security. A token issued
for you can only reach the workspaces you are an active member of; there is no cross-tenant read path.

## Reporting a security issue

Email **hello@basethread.ai**. We aim to acknowledge within two business days.
