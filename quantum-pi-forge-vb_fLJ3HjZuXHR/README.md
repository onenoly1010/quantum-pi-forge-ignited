# Quantum Pi Forge (v6)

This repository powers the Quantum Pi Forge site. It currently ships as a Create React App but includes the scaffolding for a Next.js static export; the Netlify deployment (see below) relies on the Next configuration in `next.config.mjs`.

## Prerequisites
- Node.js 18+ and `corepack` (bundled with Node 18+). `corepack` lets us invoke `pnpm` without a global install.
- One of the supported package managers (this repo uses `pnpm`).

## Getting started
1. Install dependencies:
```powershell
corepack pnpm install
```
2. Start the development server:
```powershell
corepack pnpm start
```
3. Build for production (runs the CRA-based compiler that currently drives the site):
```powershell
corepack pnpm build
```

## Preparing a Netlify export
Netlify is configured to run `pnpm build && pnpm export`. Right now the repo ships only the CRA `build` script, so the `export` step is awaiting the Next.js CLI and a matching export script (typically `"export": "next export"`). Once you have the Next CLI installed and the `export` script defined, `corepack pnpm build && corepack pnpm export` will produce the `out/` directory that Netlify publishes.

### Required `next.config.mjs` tweaks
```
output: 'export',
images: { unoptimized: true },
```
These options force Next to produce a fully static export and keep image handling simple for Netlify.

## Netlify deployment guide
| Step | Description |
| --- | --- |
| GitHub repo | `onenoly11/quantum-pi-forge-v6` |
| Build command | `pnpm build && pnpm export` (Netlify runs this in the project root) |
| Publish directory | `out` |

### Environment variables (add under Netlify Site Settings → Build & deploy → Environment)

## Netlify Guardian automation
We now ship a scheduled GitHub Action that protects `quantumpiforge.com`, watches critical validation assets, and triggers self-healing Netlify builds. Enable it by adding `.github/workflows/netlify-guardian.yml` (already provided) and setting the following repository secrets:

- `NETLIFY_AUTH_TOKEN`: Token scoped to your Netlify site for domain and build management.
- `NETLIFY_QPF_SITE_ID`: The site ID for `quantumpiforge.com` (find it in your Netlify site settings).

The workflow runs on every push plus every 15 minutes and does three things:

1. Ensures the domain is attached only to the Quantum Pi Forge site and re-assigns it if another Netlify project tries to claim it.
2. Checks that `https://quantumpiforge.com/validation-key.txt` returns `200`; if not, it kicks off a Netlify rebuild to restore the Pi Developer validation file.
3. Verifies the primary URL is healthy and redeploys automatically if it returns anything other than `200`.

This keeps your DNS, validation, and deployment health monitored continuously without manual intervention. If you’d like the Guardian Dashboard, ping me with “Enable the Guardian Dashboard.”
### Netlify & Sanity workflow
1. In Netlify, create a **Build hook** and copy the URL it returns.
2. In the Sanity Studio, add a **Webhook** that pings the Netlify build hook URL whenever content changes.
3. This keeps the exported site fresh; each webhook-triggered rebuild finishes in under 30 seconds.

## Live deploy
- `https://quantum-pi-forge-v6.netlify.app`
- Use Netlify's **Edit → Rebuild** to trigger a fresh export when needed.

## Asynchronous Pi payment flow
We surface Pi payments through two Netlify functions that write to Supabase and trigger the ethical audit once a payment completes.

### Netlify configuration
- Add `netlify.toml` with:
	```toml
	[functions]
	node_bundler = "esbuild"
	included_files = []
	external_node_modules = ["@supabase/supabase-js"]

	[functions."pi-payment-webhook"]
	included_files = []
	```
	This ensures Netlify exposes the raw request body for `pi-payment-webhook` so HMAC signing works.

### Serverless handlers
- `netlify/functions/pi-create-payment.ts` awakens a Pi payment, enriches metadata, and points the webhook at `pi-payment-webhook`.
- `netlify/functions/pi-payment-webhook.ts` verifies `x-pi-signature`, deduplicates on `payment_id`, upserts into Supabase, and only calls `ethical-audit` when `status === 'completed'` and `metadata.type === 'ethical_audit'`.
	- The handler issues structured alerts through `alert-webhook` whenever the audit or Supabase logic fails.
	- Duplicate executions short-circuit when `status === 'completed' && audit_completed === true`.

### Supabase schema
```sql
CREATE TABLE IF NOT EXISTS payments (
	id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
	payment_id TEXT UNIQUE NOT NULL,
	pioneer_wallet TEXT,
	amount NUMERIC,
	status TEXT NOT NULL,
	memo TEXT,
	metadata JSONB,
	completed_at TIMESTAMPTZ,
	audit_completed BOOLEAN DEFAULT FALSE,
	audit_score INTEGER,
	audit_resonance INTEGER,
	mining_boost INTEGER,
	created_at TIMESTAMPTZ DEFAULT NOW(),
	updated_at TIMESTAMPTZ DEFAULT NOW()
);

ALTER TABLE payments ENABLE ROW LEVEL SECURITY;
```
> Tune RLS policies to match your authentication model; the functions use the service key so no auth binding is required.

### Dashboard tips
- After `pi-create-payment` responds with `payment_id`, display `Pending` and subscribe to Supabase Realtime updates on `payments` where `payment_id = ...`.
- When the row updates to `status = 'completed' && audit_completed = true`, refresh the audit display and show any mining boost that was returned.

### Environment checklist
- `PI_PAYMENT_SECRET` (Pi webhook secret, used for HMAC SHA256).
- `SUPABASE_URL` & `SUPABASE_SERVICE_KEY` (service key for backend writes).
- `URL` (Netlify site URL, used to reach other functions).
- `TEAM_TOKEN` (internal authorization to call `ethical-audit` and `alert-webhook`).
- `PI_BACKEND_URL` (URL to your Pi backend or proxy used by `pi-create-payment`).