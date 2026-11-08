# sd-landing-page

This project defines the [Hugo](https://gohugo.io/)-generated landing page for [summittdweller.com](https://summittdweller.com/).

# Deploying This Site

This site is deployed via [DigitalOcean App Platform](https://docs.digitalocean.com/products/app-platform/), which is connected directly to this GitHub repository. There is no manual deployment step:

1. Commit and push changes to the `main` branch of this repo.
2. App Platform detects the push, clones the repo (the `themes/bootstrap-bp-hugo-startpage` theme is committed directly in this repo), builds the site with its Hugo buildpack, and serves the static output at [summittdweller.com](https://summittdweller.com/).

Note that the `baseURL` is intentionally commented out in `config.toml` so that App Platform can serve the site from whatever URL it assigns.

# Updating This Site

The process of adding a site, or making any addition/change, to this page is now very simple:

```
cd ~/GitHub/sd-landing-page
# ...edit content, config.toml, etc...
git add -A
git commit -m "Describe your change"
git push
```

App Platform *automagically* takes care of the rest!

# Analytics

The portal uses a separate GA4 property from the personal blog. Its Web stream
is `SummittDweller.com portal production website` for
`https://summittdweller.com`, with Measurement ID `G-3QPKBWP768` and stream ID
`16297878071`.

`config.toml` sets the ID under `[services.googleAnalytics]`.
`layouts/partials/head.html` emits Hugo's built-in GA4 template only in
production; `layouts/partials/js.html` does not emit a second tracker.
The Matomo site 7 snippet and obsolete Universal Analytics setting have been
removed. Do not reuse the personal blog's ID or install another Google tag.

Plain `hugo` builds are production builds. `hugo server` and builds using
`--environment development` omit analytics. Review applicable privacy and
consent requirements before publishing.

After deployment, reload the live portal and check the portal property's
Realtime report. Browser blocking can prevent visits from appearing.
The "Site Analytics" panel embeds `/analytics/report.html`, with a link to
the full report. Its generator, site configuration, tests, and scheduled
GitHub Action live in the private SummittDweller-Google-Analytics repository.
That action copies only the generated aggregate HTML into
`static/analytics/report.html`, commits it, and pushes to `main`, triggering
the usual App Platform deployment. No GA credentials are stored here or sent
to visitors. Do not add a tracker inside the report iframe.

The initial report explicitly says reporting is not configured yet; live
counts require GA4 property access and automation credentials. Once active,
reports show the last seven completed days versus the previous seven.
The update timestamp and a warning after 48 hours identify stale snapshots.
The iframe scrolls when additional sites exceed its height.
Its document-relative URL intentionally keeps the report on the serving
origin even when a Hugo build uses a default or preview base URL.

# Legacy Deployments (Retired)

This site was previously deployed as a Docker image (`summittdweller/summittdweller-landing`, later `mcfatem/summittdweller-landing`) pushed to Docker Hub and run on a DigitalOcean Docker droplet behind Traefik, with Watchtower auto-pulling new `:latest` images, and at one point built via Travis CI. Those workflows are retired; their artifacts (`Dockerfile`, `push-update.sh`, `.travis.yml`, etc.) have been preserved in the `.out-of-the-way/` directory for reference.
