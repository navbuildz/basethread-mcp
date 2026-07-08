# Publishing the BaseThread MCP registry listing

How `ai.basethread/basethread` is published to the [Official MCP Registry](https://registry.modelcontextprotocol.io)
and kept live. The registry is in **preview and can reset data**, so republishing is automated (see CI below).

> The registry's schema, TXT-record format, and `mcp-publisher` flags can change while it's in preview.
> **Verify each against the live registry docs at publish time.**

## Namespace ownership (DNS auth)

We own the `ai.basethread` namespace by proving control of `basethread.ai` with a DNS TXT record.

- **TXT record on `basethread.ai`** (host `@`):
  ```
  v=MCPv1; k=ed25519; p=<base64 ed25519 public key>
  ```
  The exact value is derived from the signing key below. **Do not delete this record** , removing it
  breaks republishing.
- **Signing key:** an Ed25519 keypair. The **private key never lives in this repo**. It is stored:
  - locally at `~/.config/basethread-mcp/signing-key.pem` (PEM) and `private-seed.hex` (32-byte hex seed),
  - and as the GitHub Actions secret **`MCP_PRIVATE_KEY`** (the hex seed) for CI republish.

## First publish (manual)

```bash
# 1. Install the publisher CLI (from modelcontextprotocol/registry tooling).
#    Verify the current install method in the live registry docs.
# 2. Authenticate via DNS with the signing key:
mcp-publisher login dns --domain basethread.ai --private-key "$(cat ~/.config/basethread-mcp/private-seed.hex)"
# 3. Publish:
mcp-publisher publish   # reads ./server.json
```

Confirm the listing is live and searchable at https://registry.modelcontextprotocol.io.

## Republish (automated)

`.github/workflows/publish.yml` republishes on any `v*` tag using the `MCP_PRIVATE_KEY` secret. Bump
`version` in `server.json`, commit, then:

```bash
git tag v1.0.1 && git push origin v1.0.1
```

## Downstream registries

Publishing to the Official Registry first is deliberate: PulseMCP, mcp.so, and Glama ingest or crawl from
it, so one correct publish cascades. Cline and Smithery may require a separate submission (link
`llms-install.md` + `SECURITY.md`).
