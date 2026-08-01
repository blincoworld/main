# Business OS V1

Unified shell for the existing Lead Control Panel and Content Control projects.

## Files

- `index.html` — shared Business OS shell and navigation
- `sales.html` — existing Lead Control Panel, preserved unchanged
- `content.html` — existing Content Control, preserved unchanged
- `content-data.json` — original 90-post seed/reset database

## Recommended deployment

Deploy this folder through the existing Content Control Cloudflare Worker project by copying all four files into its `public` directory and running:

```bash
npm run deploy
```

This keeps `GET /api/content` and `PUT /api/content` on the same origin, so Content Control retains its current Cloudflare KV storage.

## One likely backend adjustment

The Sales module calls:

`https://trade-concept-leads.pbwebonlinesales.workers.dev`

When Sales is opened from the new Business OS domain, that Worker must allow the Business OS origin in its CORS allowlist. Add the final deployed Business OS origin alongside the existing allowed origins.

For example:

```js
const ALLOWED_ORIGINS = [
  'https://piersblinco.com',
  'https://www.piersblinco.com',
  'https://blincoworld.github.io',
  'https://contentcontrol.pbwebonlinesales.workers.dev'
];
```

Use the exact deployed hostname if it differs.

## V1 design decision

Sales and Content are displayed as isolated same-project modules. This preserves both working codebases and avoids a risky rewrite. The next sensible iteration is a shared authentication layer and a real Overview API that combines CRM, MailerLite, calendar and content metrics.
