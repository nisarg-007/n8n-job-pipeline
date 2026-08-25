# n8n on Render — Job Pipeline Deploy

One-file Render Blueprint. Deploys n8n (web) + Postgres, both on Render free tier.

## What this provisions

- **n8n web service** — Docker `n8nio/n8n:latest`, free plan.
- **Managed Postgres** — Render free plan (1 GB, 90-day trial then auto-expires).
- **Basic auth** — auto-generated password shown in Render env vars after first deploy.
- **Timezone** — pinned to `America/Chicago` so cron fires at your local 09:00 / 15:00 / 21:00.
- **Anthropic key** — read from `ANTHROPIC_API_KEY` env var (you set this once in Render dashboard).

## Known free-tier limits (unbiased)

1. **Render web service sleeps after 15 min idle.** The internal n8n cron will NOT fire while asleep. Fix: set up cron-job.org to GET `https://n8n-<random>.onrender.com/healthz` every 10 min (free). Instructions in the main setup guide.
2. **Render Postgres free plan expires after 90 days.** Data (workflows, credentials) is wiped. Options at that point: recreate DB, migrate to Neon.tech / Supabase free Postgres, or upgrade to $7/mo Render Postgres.
3. **First cold start after sleep = ~30-60s.** The 5-min wait node in the workflow absorbs this.
