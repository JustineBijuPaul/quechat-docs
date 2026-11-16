# Organizations and workspaces

## Hierarchy

`Platform → Organizations → Workspaces → AI resources`

- An **organization** is an isolated tenant (can be active, archived, or soft-deleted).
- A **workspace** owns agents, knowledge, languages, providers, memory, widget theme, API keys, analytics, and logs for one product/site context.

Resources in one workspace are not shared with another workspace.

## Public workspace key

Each workspace has a `publicKey` used by legacy embeds (`?workspaceKey=`).

Seeded demo workspace uses `publicKey=default`.

Prefer **API keys** (`qc_live_…`) for production installs. See `10-api-keys.md` and `11-embedding-overview.md`.

## Creating and deleting workspaces

- Only the **Owner** can create or delete workspaces.
- Owner can put an **Admin** in charge of a workspace.
- **Admin** and **Member** can manage workspace resources (agents, knowledge, memory, widget, etc.) but cannot create/delete workspaces.
- **Read-only** can view, not edit.

## Admin context switching

In `/app`, operators switch organization and workspace. Resource APIs then use:

- `x-organization-id`
- `x-workspace-id`

Always confirm the active workspace before editing providers, knowledge, or API keys.

## Signup defaults

Owner signup creates one organization and one default workspace. Additional workspaces are created from the portal when the Owner needs separate sites, brands, or environments (for example staging vs production).
