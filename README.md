# cron

Scheduled calls to the Islamic Tech Collective's services. Vercel's Hobby plan runs cron jobs at most
once a day, so each service's `vercel.json` keeps only daily jobs, and the frequent ones run here,
from [`.github/workflows/schedule.yml`](.github/workflows/schedule.yml):

| Every | Calls |
|---|---|
| 5 minutes | `status/api` `/cron/checks`, `email/api` `/cron/resume-campaigns` |
| 15 minutes | `portal/api` `/cron/claims` |
| hour | `portal/api` `/cron/governance` |

Each call is `GET <service>/cron/<job>` with `Authorization: Bearer <CRON_SECRET>`, the same request
Vercel Cron makes. Nothing secret is in this repository: the addresses and the secret are repository
secrets (Settings > Secrets and variables > Actions), listed at the top of the workflow. A service
whose URL isn't set is skipped.

The repository is public because GitHub Actions minutes are free for public repositories, and a job
every five minutes would use up a private repository's allowance. GitHub pauses schedules in a
repository with no activity for 60 days; re-enable the workflow from the Actions tab if it stops.
