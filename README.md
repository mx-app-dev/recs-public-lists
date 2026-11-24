# recs-public-lists

The read-only page for public lists of the Recs2 app. A list is public when its owner has shared it by link. The page
shows the list for the link's token, and nothing else.

## Where the files are

- `site/lists/index.html`: the page. It is served at `/lists/`, because the app builds links to that path.
- `site/config.js`: placeholders only. The deploy workflow replaces it.
- `.github/workflows/deploy.yml`: publishes the `site` folder to GitHub Pages.

## Where the Supabase values go

Set them as repository secrets, never in a file:

1. Repository **Settings → Secrets and variables → Actions → New repository secret**.
2. `SUPABASE_URL`: the project URL, `https://<project-ref>.supabase.co`.
3. `SUPABASE_PUBLISHABLE_KEY`: the project's publishable (anon) key from the Supabase dashboard. Never the service-role key.

The deploy workflow writes these into `site/config.js` when it publishes. The publishable key is public by design: any
visitor can read it in the page. What protects the data is the database (migration 0021): the key can only call
`get_public_list`.

## Turn on GitHub Pages once

**Settings → Pages → Build and deployment → Source: GitHub Actions.**

After the first deploy, the page is at `https://mx-app-dev.github.io/recs-public-lists/lists/?t=<token>`.

## What the page shows

Per recommendation: title, creator, year, category and cover. Nothing else. Status, rating, the recommender and the
private note are never sent by the database function, so the page cannot show them.

## Notes

- The token is in the page address, so GitHub's logs and the visitor's own history contain it. The page sets
  `referrer` to `no-referrer`, so cover images do not receive it.
- An unknown token and a stopped one show the same message, on purpose.
- Cover images come from the catalog's sources; whether they may be shown is an open legal question (`docs/LEGAL.md`, question 8).
