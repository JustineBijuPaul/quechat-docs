# Invites and users

## Inviting teammates

Owner or Admin users invite people from the portal **Users** area:

1. Enter the invitee email.
2. Choose a role (must be **strictly below** your own).
3. Invite is scoped into the current workspace (org + workspace roles stay paired).
4. Share the invite URL, or rely on email delivery when configured.

Invite links look like `/invite/[token]` and expire (typically ~14 days).

## Email delivery

If `RESEND_API_KEY` is set (optional `EMAIL_FROM`), QueChat can send the invite email.

Without email configured, copy the invite URL from Admin and send it manually.

## Accepting an invite

1. Open the invite link.
2. Set a password.
3. Log in at `/login` with that email and password.
4. Land in `/app` with the assigned role.

## Role pairing

Organization and workspace roles are kept aligned. When an invite or role update sets an org role, the workspace role follows the mapping used by QueChat (and vice versa where applicable).

## Who can manage users

- **Owner** and **Admin** can manage users and send invites.
- They may only assign roles **strictly below** their own.
- **Member** and **Read-only** cannot manage users.
- **Super Admin** has platform-wide control.

See `05-roles-permissions.md` for the full hierarchy and capability matrix.
