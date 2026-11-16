# Auth, signup, and login

## Owner signup

1. Open `/signup`.
2. Register with **Google** or email and password.
3. Provide an organization billing profile:
   - Organization name
   - Your name
   - Phone number
   - Address (city and country optional)
   - Website (optional)
   - Password (email signup, or after Google so you can also log in with email)
   - Consent to use these details for invoices, billing, and support
4. QueChat creates:
   - An **Owner** user
   - A new **organization** (with `billingConsentAt` when consent is given)
   - A default **workspace**

Google register verifies email first, then redirects to `/signup/complete` to collect the same profile fields.

Owners may have only **one organization**, but can create **multiple workspaces**.

## Login

1. Open `/login`.
2. Sign in with Google or email and password.
3. Successful login redirects to `/app`. If Google created an account that still needs a billing profile, the user is sent to `/signup/complete`.

Sessions are cookie-based and require `SESSION_SECRET` in the environment.

## Google OAuth

Set `GOOGLE_AUTH_CLIENT_ID` and `GOOGLE_AUTH_CLIENT_SECRET` (or reuse `GOOGLE_OAUTH_CLIENT_ID` / `GOOGLE_OAUTH_CLIENT_SECRET`).

Authorized redirect URI:

`{APP_URL}/api/auth/google/callback`

Scopes: `openid email profile`.

## Super Admin

There is exactly one platform **Super Admin** (not an org/workspace role you assign via invite).

- Default seeded email: `admin@quechat.dev` (override with `SUPER_ADMIN_EMAIL`)
- Password: `SUPER_ADMIN_PASSWORD` (default often `quechat-admin` in local/demo setups)

Super Admin signs in at `/login` like everyone else, then uses `/app` with platform-wide access.

## Invitees

Teammates do not self-serve signup into an existing org. They:

1. Receive an invite link (`/invite/[token]`)
2. Set a password
3. Log in at `/login`

See `03-invites-and-users.md`.

## Demo users (local / seed)

Shared default password is typically `quechat-admin` (`DEMO_PASSWORD` / `SUPER_ADMIN_PASSWORD`):

| Role | Email |
|------|-------|
| Super Admin | `admin@quechat.dev` |
| Owner | `owner@quechat.dev` |
| Admin | `org-admin@quechat.dev` |
| Member | `member@quechat.dev` |
| Read-only | `readonly@quechat.dev` |

Change these credentials before any public deployment.
