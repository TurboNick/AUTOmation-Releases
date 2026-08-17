# AUTOmation — releases

Built plugin packages and their update manifests. **No source code lives here** —
this repo exists so that WordPress can find updates.

The three AUTOmation plugins are not on wordpress.org, so WordPress has nowhere to
check for new versions. Each plugin declares an `Update URI:` pointing at its
manifest in this repo and reads it on the Updates screen, which turns a release
into a one-tap **Update** button in wp-admin — the only way the production site
can be deployed to, since it has no SSH or SFTP access.

| Plugin | Manifest |
|--------|----------|
| AUTOmation (core) | [`automation.json`](automation.json) |
| AUTOmation for Wave | [`automation-wave.json`](automation-wave.json) |
| AUTOmation Comms | [`automation-comms.json`](automation-comms.json) |

## Why this repo is public

The manifests are fetched by a live WordPress site with no credentials of its own.
A public `raw.githubusercontent.com` URL needs no token, which keeps API keys off
the production server entirely — the safest place for a secret being no secret at
all.

Nothing sensitive ships here: the packager excludes every `.env`, so no API token
or password is inside these archives.

## Publishing

Never by hand. `release.ps1` in the core repo publishes here, and only after every
plugin has passed lint, deploy and its full test suite against a real WordPress
database. Committing a zip directly would bypass that gate and could push an
unverified plugin onto a live site.

Old versions are kept deliberately, so a release can be rolled back by pointing a
manifest at an earlier zip.
