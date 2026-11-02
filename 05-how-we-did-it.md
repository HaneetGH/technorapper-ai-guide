# 5. How we did it

This is the method, not a diary. Use it on another WordPress site. Swap the URL.

## The site we started from

Technorapper is a tech news site. It has been public since 2012. By 1 November 2026 it held about 134,000 stories, most of them imported through RSS. Categories 404’d. A spinner could sit on an article for a minute. Ads sat in the wrong place. Feeds often brought a line or two. Plugins had been installed and left on.

The archive stayed. The URLs stayed. The AdSense publisher ID stayed.

## 1. Connect to the live site, then prove it

Install [WPVibe](https://wpvibe.ai) on WordPress. Point Cursor at `https://mcp.wpvibe.ai/mcp` ([MCP setup](02-mcp-setup.md)).

The first prompt is only `site_info`. If the name or URL is wrong, stop. You are on the wrong site.

Do not paste the WordPress password into the chat. The connector sends a one-click approve link.

## 2. Audit with the tools, not with a guess

Ask for a full read-only audit: health, plugins, theme, SEO, ads, feeds, performance. Tell the model not to invent Lighthouse scores or traffic.

Write the findings down before any repair. We kept ours in a private notes repo. This public repo is the playbook.

Ask for a short fix list. Approve that list, not “fix everything.”

## 3. Work in batches, with a backup already taken

Confirm a backup you can restore (host snapshot or a backup plugin you already run). Do not let the model install a backup plugin as part of the repair.

Each batch had a hard stop:

- Do not delete posts.
- Do not change permalinks.
- Do not change the AdSense publisher ID.
- Do not upgrade a parent theme we had already patched.

Database work went through the plugin’s own path (ability, REST, WP-CLI). Raw SQL was last, not first. Some options still belong in wp-admin: on this site, `home` and `siteurl` could not be switched to HTTPS through the tool.

## 4. Fix content and crawl before the theme

On this site that meant:

- Strip a JSON wrapper off imported posts that were publishing as source code.
- Rebuild categories and menus from the actual feed sources. Do not invent new slugs for old URLs.
- Point imports at feeds that carry a full article. Reject feeds that only return a sentence.
- Replace a dead `robots.txt` sitemap with the real one.
- Write a homepage description that is not the tagline.
- Put a working contact form on the contact page.
- Turn off unused plugins in the database. Leaving the folder on disk is not the same as leaving the plugin on.

None of that needs a new theme. Do it first so you are not restyling a broken information architecture.

## 5. Change a few files, upload those files

A downloaded `public_html` is a snapshot. Ours was taken before the audit. Uploading the whole folder would have put old plugins and `wp-config.php` back on the live site.

List the paths. Upload only those paths. Then request the public URL with a cache-buster and search the HTML for the string you changed. Details: [public_html](03-public-html.md).

The first theme work was patches on CoverNews (preloader, headings, “Read More”, ticker overflow). We did not upgrade that parent in place. An upgrade would have overwritten the patches.

## 6. When the parent theme is the bottleneck, ship a small theme

CoverNews still pulled a large CSS/JS stack and large remote images. We replaced it with a small custom theme: [technorapper-wp-theme](https://github.com/HaneetGH/technorapper-wp-theme).

What mattered for LCP on a feed-driven homepage:

- The first image is the LCP. Serve it at the size the layout uses (here, 800×450), with `fetchpriority="high"` and a preload.
- Other above-the-fold tiles should be `eager`. Lazy-loading them left empty boxes.
- Off-site RSS images can be 2000px wide. Send them through a resizer (we used WordPress.com Photon: `i0.wp.com`) instead of hotlinking the original.
- Load Disqus comment counts only on single posts.
- Keep the same AdSense client. Put the script in the footer if Site Kit is not already injecting it.

Do not treat a custom theme as a rewrite of the business. Permalinks, publisher ID, and the post table stay.

## 7. Purge the edge cache, then measure the page visitors get

This host runs nginx in front of Apache. `X-Proxy-Cache: HIT` means you are scoring yesterday’s HTML.

Purge, then fetch until you see `MISS` or `EXPIRED`. Then run [PageSpeed Insights](https://pagespeed.web.dev/) on the public URL. Local Lighthouse on a laptop is useful. It is not the same lab as PSI.

How we treated numbers: [How we measured](04-measure.md).

## 8. Say what still is not done

A playbook that only lists wins is a press release. On this site, after the PSI run:

- Chrome UX Report still has no field data.
- A cache-miss homepage can still take about two seconds to first byte. There is no CDN on the apex hostname.
- WordPress Settings → General may still show `http` even when the public site is HTTPS. Change that in wp-admin. The MCP tool refused that option write.
- Ads still add third-party JavaScript. That is a publisher choice, not a theme bug.

## What to paste when you do this yourself

The prompts in [01-prompt.md](01-prompt.md) are the whole operating system. The line that kept this site intact was:

```text
Do not delete posts. Do not change permalinks.
Do not change the AdSense publisher ID.
Do not invent a Lighthouse score.
```
