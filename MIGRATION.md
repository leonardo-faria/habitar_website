# Deploy this repo to jornalhabitar.pt

The Pages project and domain already exist. **A deploy replaces the live site
immediately** — no prompt. Roll back from the Cloudflare dashboard.

## One-time setup

```bash
gh secret set CLOUDFLARE_ACCOUNT_ID --body <TEAM_ACCOUNT_ID> --repo leonardo-faria/habitar_website
gh secret set CLOUDFLARE_API_TOKEN --repo leonardo-faria/habitar_website
gh variable set CF_PROJECT --body <PROJECT> --repo leonardo-faria/habitar_website
```

Token needs `Account -> Cloudflare Pages -> Edit`, with the account added under
*Account Resources*. Find `<PROJECT>` in the dashboard under
Workers & Pages. The workflow reads the
project name from the `CF_PROJECT` repo variable, so no code edit is needed.

If the project's source type is **not** `direct-upload`, it is already wired to
a repo: dashboard -> Workers & Pages -> project -> Settings -> Builds &
deployments -> Disconnect, or the two will fight.

## Deploy

```bash
gh workflow run "Deploy to Cloudflare Pages" --repo leonardo-faria/habitar_website
```

Or push to `main` — same workflow. Or from a phone: github.com -> Actions ->
Deploy to Cloudflare Pages -> Run workflow.

To list projects or roll back, use the dashboard: Workers & Pages -> project ->
Deployments -> "..." -> Rollback.

## Verify

```bash
gh run list --repo leonardo-faria/habitar_website --limit 1
curl -s -o /dev/null -w "%{http_code} %{size_download}\n" "https://jornalhabitar.pt/?cb=$RANDOM"
```

## Clean up the test project

Personal account, different token: Workers & Pages -> `habitar` -> Settings ->
Delete project.
