# menacebench-results

Submitted [menacebench](https://github.com/bitplane/menacebench) NetHack runs,
published at **https://menace.bitplane.net**.

Each run is one folder, `attempts/<attempt-id>/`, holding its run package:

- `attempt.json`: the manifest (player, character, outcome, score, image digest, …)
- `events.jsonl.gz`: the seed-relative event log, including every key the game consumed

Packages are submitted from a menacebench server's dashboard (**Submit to
results** on a finished run); the server opens the pull request with the admin's
account and credits the submitter. Packages are never edited after they are
merged. A maintainer may add `attempts/<attempt-id>/disqualified.json`
(`{"reason": "..."}`) for a run that broke the player rules; it stays listed on
the site's disqualified page and leaves the rankings.

## Checks

`Check submission` runs on every pull request with no secrets and a read-only
token. It rejects changes outside new package files, validates each package's
schema and digests, and replays packages recorded in a published official image
(listed in menacebench's `config/official-images.json`) inside that image, with
networking off. Results:

- **verified**: reproduced in its recorded official image; ranked
- **unverified**: recorded with a development build or an unpublished image, so
  CI cannot replay it; listed but not ranked
- **failed**: invalid or does not reproduce; do not merge

`Publish site` rechecks every package on `master` and deploys the site to GitHub
Pages.

## Maintainer setup

- Settings → Pages: source **GitHub Actions**, custom domain `menace.bitplane.net`
  (DNS: `CNAME menace` → `bitplane.github.io`), enforce HTTPS.
- Settings → Actions: require approval for workflows from outside contributors.
- Protect `master` and require the `Check submission` job.
