# Branding — QueChat powered by BluVise

QueChat’s product brand lockup is:

**QueChat powered by BluVise**

BluVise website: https://www.bluvise.com/

## Where branding appears

- Marketing homepage
- Public docs (`/docs`)
- Login, signup, and invite pages
- Workspace portal sidebar / brand kicker
- HTML metadata titles and descriptions where configured
- Optional widget branding toggle (`showBranding`)

Implementation lives in `components/BrandLockup.tsx` (and its CSS module).

## Guidance for answers and docs

When describing the product:

- Call the product **QueChat**
- Attribute it as **powered by BluVise**
- Link BluVise to https://www.bluvise.com/ when a company reference is useful
- Do not invent alternate parent brands

Widget-level “QueChat” chrome inside customer sites is controlled per workspace via Admin → Widget.
