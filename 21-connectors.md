# Action Engine connectors

Full setup guides (base URLs, API keys, scopes, and step-by-step credentials) live on the docs site:

**[/docs#connectors](/docs#connectors)**

Install connectors in **Admin → Connectors**, then enable actions in **Admin → Actions**. Chat tools only call the Action Engine — never external APIs directly.

## Catalog

| Connector | Auth | Example actions |
|-----------|------|-----------------|
| Generic REST | API key / Bearer / Basic | `rest_get`, `rest_post`, `rest_request` |
| Webhook | Optional Bearer | `webhook_send` |
| Email (Resend) | API key | `email_send`, `email_draft` |
| Google Calendar | OAuth access token | `calendar_schedule_meeting`, `calendar_find_free_time`, … |
| Microsoft Outlook | Graph token | `outlook_schedule_meeting`, `outlook_send_email` |
| Slack | Bot token | `slack_post_message` |
| Microsoft Teams | Incoming webhook URL | `teams_post_message` |
| Discord | Webhook URL or bot token | `discord_post_message` |
| WhatsApp Cloud | Meta token + phoneNumberId | `whatsapp_send_message` |
| GitHub | PAT | `github_list_repos`, `github_get_repo`, `github_list_issues`, `github_list_pull_requests`, `github_create_issue`, `github_comment_issue` |
| HubSpot | Private app token | `crm_create_contact`, `crm_create_deal`, `crm_create_lead` |
| Salesforce | Instance URL + token | `salesforce_create_lead`, `salesforce_create_contact` |
| Google Drive | OAuth token | `storage_upload_file`, `storage_delete_file` |
| HTTP Database Gateway | Bearer/API key | `database_read`, `database_insert` |

## Make the chatbot perform a task

1. Install the connector and paste credentials.
2. Keep the action **enabled** (Actions tab).
3. Ensure chat tools are on (`toolsEnabled`).
4. Ask in natural language, e.g. “Schedule a meeting tomorrow at 10am with alex@acme.com”.

Sensitive actions (delete meeting, REST DELETE, Drive delete) may pause for **Approvals**.

## Tokens

OAuth browser flows are not built yet. For Google / Microsoft / Slack, create a token in the provider console (or OAuth playground) with the needed scopes and paste it into Connectors → Configure.

Reseed catalog after upgrades:

```bash
npx prisma db seed
# or Docker: RUN_DB_SEED=true docker compose up
```
