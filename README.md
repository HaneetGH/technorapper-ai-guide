# Repair a live WordPress site with AI

This is the public playbook from a weekend spent on [technorapper.com](https://technorapper.com): a technology site that has run since 2012.

Copy it. Change the URL. Keep our passwords, database, and `wp-config.php` out of your repo.

The theme that ended up live is separate: [technorapper-wp-theme](https://github.com/HaneetGH/technorapper-wp-theme). This repository is the method.

## Who this is for

You have a WordPress site that grew by adding plugins and RSS feeds. It still ranks, it still makes money, and you have never sat down to clean it. You want an agent in Cursor to read the live install, not a guess from a screenshot.

You do not want it to delete the archive, rewrite permalinks, or invent a Lighthouse score.

## How to use this repo

1. Fork it, or clone it next to your own notes.
2. Connect Cursor to the live site with [WPVibe](https://wpvibe.ai) ([MCP setup](02-mcp-setup.md)).
3. Paste the prompts in [the prompts](01-prompt.md), in order. The first one is read-only on purpose.
4. Approve work in small batches. Upload only the files you changed ([public_html](03-public-html.md)).
5. Publish only numbers you measured ([how we measured](04-measure.md)).

The story of what we actually did on this site is [how we did it](05-how-we-did-it.md).

Read in order: [prompts](01-prompt.md) · [MCP](02-mcp-setup.md) · [files](03-public-html.md) · [measure](04-measure.md) · [the weekend](05-how-we-did-it.md).

## What the labs showed

Two different labs. Do not write them as one chart.

**1–2 November, mobile Lighthouse on the homepage** (WPVibe, CoverNews still live). One run each day. No Chrome UX Report field data.

| | 1 November | 2 November |
| --- | ---: | ---: |
| Performance | 41 | 69 |
| Accessibility | 90 | 89 |
| SEO | 85 | 85 |
| Largest Contentful Paint | 13.8 s | 16.8 s |
| Cumulative Layout Shift | 0.387 | 0.005 |
| First Contentful Paint | 2.5 s | 1.7 s |
| Speed Index | 9.6 s | 5.1 s |
| Total Blocking Time | 210 ms | 140 ms |

Layout shift got much smaller. Performance rose. The main image got slower.

**Later on 2 November, official PageSpeed Insights, mobile lab**, after a custom theme and smaller homepage images. Lighthouse 13.6.0, emulated Moto G Power, slow 4G. Chrome UX Report still **No Data**.

| | PSI |
| --- | ---: |
| Performance | 90 |
| Accessibility | 100 |
| SEO | 100 |
| First Contentful Paint | 1.0 s |
| Largest Contentful Paint | 2.7 s |
| Total Blocking Time | 150 ms |
| Speed Index | 5.5 s |
| Cumulative Layout Shift | 0 |

Report: [pagespeed.web.dev/analysis/https-technorapper-com/rd9rm04l37](https://pagespeed.web.dev/analysis/https-technorapper-com/rd9rm04l37?form_factor=mobile)

Do not write 69 → 90 as if it were one continuous lab. The honest LCP line across both days is **13.8 s → 16.8 s → 2.7 s**.

AIOSEO’s own checklist later showed 83. That is not a Google score. Do not mix it with PageSpeed Insights.

## Rules we did not break

- Read the site before changing it.
- Do not delete the post library to “clean up” an import.
- Do not change the permalink structure.
- Do not change the AdSense publisher ID.
- Do not upload a whole old `public_html` over the live site.
- Do not commit `wp-config.php`, database dumps, uploads, or passwords.
- Do not publish a score you did not measure.
- If a number got worse, say so.

AI was useful because it could read the theme, the plugins, the feeds, and the rendered HTML, then change a small set of files and check the result. It did not replace a backup, a host, or a stopwatch.

## License

The playbook is [MIT](LICENSE). The WordPress theme is GPL-2.0-or-later, in its own repo.
