# Nine Lives — deploy notes

Static site. No backend, no build step, no dependencies. Drop the folder on any host.

## Deploy

1. Put this folder in a GitHub repo.
2. Connect it to Cloudflare Pages or Netlify. Build command: none. Output directory: `/`.
3. Add your domain in the host dashboard. HTTPS provisions automatically.

`_headers` works as-is on both Cloudflare Pages and Netlify. On other hosts, port the
same headers to their config format.

## Before you go live

- [ ] Replace `https://example.com` in the `og:url` meta tag with your real domain.
- [ ] Trademark check on the name "Nine Lives" (USPTO TESS), plus domain and App Store.
- [ ] Add cookieless analytics — Cloudflare Web Analytics or Plausible. Avoid GA4 unless
      you want to write a cookie banner and privacy policy.
- [ ] Decide whether the default language setting should be Salty or Clean. Currently
      Salty; users can switch in the stats panel and the choice persists.

## Frozen constants — do not change after launch

| Thing | Where | Why |
|---|---|---|
| `EPOCH` | top of script | Renumbers every puzzle for everyone |
| `hash()` / `mulberry32()` / `build()` | script | Changes today's deck mid-day, breaks comparability |
| `K` storage keys | top of script | Renaming wipes every player's streak and stats |

Everything else — design, quips, tier bands, new modes — is safe to iterate on.
`index.html` is served with `must-revalidate`, so updates reach returning players immediately.

## Known limits

- Daily rollover is at **local** midnight, so timezones are on different puzzle numbers
  simultaneously. Same as Wordle. Change to UTC in `todayKey()` if you'd rather.
- The seed is computed client-side from the device clock, so the daily is trivially
  bypassed by changing the system date. Fine for a friends game; a real leaderboard
  would need a server.
- Storage is `localStorage` only. Private browsing disables streaks; the game detects
  this and says so rather than failing silently.

## Data collected

None. No accounts, no cookies, no network calls. Everything is in the visitor's own
browser storage, and the stats panel has an erase button.
