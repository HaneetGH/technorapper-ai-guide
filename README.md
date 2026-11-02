# Fix a WordPress site with AI

Technorapper has been a technology site since 2012. By 2026 it held about 134,000 stories, the homepage was hard to use, ads sat in the wrong places, and opening one article could leave a spinner on screen for more than a minute.

We repaired it in Cursor, with the site connected through [WPVibe](https://wpvibe.ai) so the model could read the live WordPress install instead of guessing. This repository is the playbook: the prompts, the MCP setup, and what to do with a downloaded `public_html`. Copy it for your own site. Change the URL. Leave our passwords, database, and theme files out of it.

The live site is [https://technorapper.com](https://technorapper.com).

## What the lab actually showed

Mobile Lighthouse on the homepage. One run each day. No Chrome UX Report field data, so these are lab numbers. They move several points between runs.

| | 1 November | 2 November |
| --- | ---: | ---: |
| Performance | 41 | 69 |
| Accessibility | 90 | 89 |
| Best practices | 96 | 100, then 81 on the last run |
| SEO | 85 | 85 |
| Largest Contentful Paint | 13.8 s | 16.8 s |
| Cumulative Layout Shift | 0.387 | 0.005 |
| First Contentful Paint | 2.5 s | 1.7 s |
| Speed Index | 9.6 s | 5.1 s |
| Total Blocking Time | 210 ms | 140 ms |

The layout shift got about 77 times smaller (0.387 down to 0.005). Performance rose 28 points. Best practices reached 100 on two runs. The last run scored 81 because Chrome flagged one deprecated API.

SEO stayed at 85. The homepage HTML no longer has the “Read More” and `javascript:` links those checks named, and a Lighthouse run after that last upload was not taken, so we do not claim a higher SEO score.

The main image got slower, not faster. A cold homepage still takes about 2.5 seconds before the first byte. We did not measure requests per second, and we do not claim the server got faster.

## Read these in order

1. [The prompts](01-prompt.md) — what to paste, and the order that kept the site safe.
2. [MCP setup](02-mcp-setup.md) — connect Cursor to the live site without putting a password in the chat.
3. [public_html](03-public-html.md) — the downloaded files are a snapshot. Upload only what you changed.
4. [How we measured](04-measure.md) — so the next person does not invent a bigger number.

## Rules we did not break

- Read the site before changing it.
- Do not delete the post library to “clean up” an import.
- Do not change the permalink structure.
- Do not change the AdSense publisher ID.
- Do not upload a whole old `public_html` over the live site.
- Do not commit `wp-config.php`, database dumps, uploads, or passwords.
- Do not publish a score you did not measure.

AI was useful because it could read the theme, the plugins, the feeds, and the rendered HTML, then change a small set of files and check the result. It did not replace a backup, a host, or a measurement.
