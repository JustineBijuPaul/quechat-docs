# Roles and permissions

QueChat uses a strict role hierarchy.

## Hierarchy

`Super Admin → Owner → Admin → Member (Edit) → Read-only (View only)`

Higher roles can manage lower ones. Assigners may only grant roles **strictly below** their own.

## Role summary

| Role | Scope | Typical capabilities |
|------|-------|----------------------|
| Super Admin | Platform | Full platform access; not an org invite role |
| Owner | Org + workspaces | Create/delete workspaces, manage users, all resources, API keys |
| Admin | Org / workspace | Manage users (below Admin), manage resources; no workspace create/delete |
| Member | Workspace | Edit/view workspace resources; **no** create/delete of many resources; **no** API key manage |
| Read-only | Workspace | View only |

UI labels often show Member as **Edit** and Read-only as **View only**.

## Notable capability rules

- **API keys** (create / rotate / delete): Admin, Owner, Super Admin only.
- **Create / delete** agents, knowledge docs, providers, etc.: restricted so Members can edit/view but not create/delete in many flows.
- **Widget configuration:** Super Admin, Owner, Admin, and Member can edit theme (Read-only cannot).
- **Workspace create/delete / assign workspace Admin:** Owner (and Super Admin).

## Permission keys (examples)

Permissions are defined in `lib/permissions.ts`, including families such as:

- Organization: view/create/update/delete, manage members
- Workspace: view/create/update/delete, manage members
- Agents, knowledge, languages, memory, LLM providers
- API keys, SDK generate, widget configure
- Analytics and audits

Exact grants depend on the system role mapping for each hierarchy level.

## Practical guidance

1. Use **Owner** for the account that owns the business tenant.
2. Use **Admin** for day-to-day operators who should manage users and keys.
3. Use **Member** for editors who configure agents/knowledge/theme but should not mint API keys.
4. Use **Read-only** for stakeholders who only need visibility.
