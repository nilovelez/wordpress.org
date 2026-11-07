# WordPress.tv Theme (wptv-27)

Block theme for [WordPress.tv](https://wordpress.tv/), the WordPress community site that publishes videos from WordCamps, meetups and other community events.

wptv-27 replaces the legacy classic theme (`wptv2`). It is built with Full Site Editing (block templates, template parts, patterns and `theme.json`), and relies on WordPress core features instead of hand-written code wherever possible.

> **Status:** Release 1 in development, theme version 0.4.2. The block-theme layouts, automatic WordCampTV landing, locked video editor, anonymous submission form and per-field moderation are implemented. R1/R2/R4 security fixes and upload-format changes are integrated and passed focused Studio checks with imported data. The latest full 145-URL comparison predates those fixes: all recorded route/query fields match, but it is **not an exact-match pass** because four poster-related feed hashes remain unaccepted; four API hashes have existing allowances. A post-fix route run remains pending authorization.
>
> **Release 1 establishes a conservative block-theme baseline in production**, preserving existing behavior except for explicitly approved changes. Uploads have already been tested and do not work on the Atomic test site, so that environment cannot validate the complete upload path. Final platform-dependent checks must take place in production, in an owner-approved window with rollback to `wptv2` and an agreed observation period. That baseline is not established yet; further functional development follows it. Subtitle player integration, upload and incoming-subtitle management are deferred to the next delivery. Git publication is not deployment. See the [regression report](https://github.com/nilovelez/wordpress.org/blob/wptv-27-dev/wordpress.tv/public_html/wp-content/themes/wptv-27/docs/regression-editor-checkpoint.md), [session handoff](https://github.com/nilovelez/wordpress.org/blob/wptv-27-dev/wordpress.tv/public_html/wp-content/themes/wptv-27/docs/handoff.md) and [project plan](https://github.com/nilovelez/wordpress.org/blob/wptv-27-dev/wordpress.tv/public_html/wp-content/themes/wptv-27/docs/plan.md).

> **Development branch:** the theme is developed on the [`wptv-27-dev` branch](https://github.com/nilovelez/wordpress.org/tree/wptv-27-dev/wordpress.tv/public_html/wp-content/themes/wptv-27) of the [nilovelez/wordpress.org](https://github.com/nilovelez/wordpress.org) fork of the Meta repository. That branch adds the development files listed below; the copy in the Meta repository is a `git archive` export without them. The links and commands in this README refer to that branch. The earlier standalone repository, [nilovelez/wptv-27](https://github.com/nilovelez/wptv-27), is archived.

## Constraints

WordPress.tv runs on the WordPress.com multisite. That means:

- No custom plugins can be installed. Only the plugins available on the multisite can be used, so every site-specific feature lives in this theme.
- Videos are hosted on VideoPress.
- Several VideoPress helpers only exist on WordPress.com. They are always called behind `function_exists()` checks, so the theme also runs in local environments.

See [`docs/wpcom-constraints.md`](https://github.com/nilovelez/wordpress.org/blob/wptv-27-dev/wordpress.tv/public_html/wp-content/themes/wptv-27/docs/wpcom-constraints.md) for details.

## Repository layout

| Path | Purpose | In theme zip |
|---|---|---|
| `AGENTS.md` | Entry point for AI coding agents: project rules and documentation index | No |
| `docs/` | Project documentation: handoff, plan, route inventory, legacy analysis, decisions | No |
| `tests/blocks/`, `tests/routes/` | Local-only block/API checks and route regression tooling | No |
| `tests/*.php`, `phpunit.xml.dist` | PHPUnit suite run by the Meta repository's WordPress.tv environment and CI (`npm run wptv:test` from its `environments/` folder) | Yes |
| `composer.json`, `phpcs.xml.dist` | WordPress Coding Standards (`composer lint`); dependencies install to the ignored `vendor/` | No |
| `blueprint.json` | Empty Playground/Studio review site for standards, security and WP-CLI checks | No |
| Everything else | The theme itself | Yes |

## Working on the theme

Make a sparse clone of the fork with only WordPress.tv (and the Meta environments), on the development branch:

```bash
git clone --filter=blob:none --sparse --branch wptv-27-dev https://github.com/nilovelez/wordpress.org.git wordpress.org-wptv
cd wordpress.org-wptv
git sparse-checkout set wordpress.tv environments
git remote add upstream https://github.com/WordPress/wordpress.org.git
```

Then link the theme folder into the `wp-content/themes/` directory of a local WordPress site (for example, a [WordPress Studio](https://developer.wordpress.com/studio/) site):

```bash
ln -s /path/to/wordpress.org-wptv/wordpress.tv/public_html/wp-content/themes/wptv-27 wp-content/themes/wptv-27
```

Bring in changes committed to the Meta repository (for example by Meta committers) with:

```bash
git fetch upstream
git merge upstream/trunk
```

Read [`AGENTS.md`](https://github.com/nilovelez/wordpress.org/blob/wptv-27-dev/wordpress.tv/public_html/wp-content/themes/wptv-27/AGENTS.md) and the [`docs/`](https://github.com/nilovelez/wordpress.org/tree/wptv-27-dev/wordpress.tv/public_html/wp-content/themes/wptv-27/docs) folder before making changes. The most important rule: **existing URLs must not change**. See [`docs/routes.md`](https://github.com/nilovelez/wordpress.org/blob/wptv-27-dev/wordpress.tv/public_html/wp-content/themes/wptv-27/docs/routes.md).

## Building a clean theme zip

Development-only files (`docs/`, `tests/blocks/`, `tests/routes/`, `AGENTS.md`, `.gitattributes`, `.gitignore`, `.github/`, `blueprint.json`, `composer.json`, `composer.lock`, `phpcs.xml.dist`) are marked `export-ignore` in [`.gitattributes`](https://github.com/nilovelez/wordpress.org/blob/wptv-27-dev/wordpress.tv/public_html/wp-content/themes/wptv-27/.gitattributes), so `git archive` leaves them out.

From the clone, on the development branch:

```bash
git archive --format=zip --prefix=wptv-27/ --output=wptv-27.zip HEAD:wordpress.tv/public_html/wp-content/themes/wptv-27
```

- `HEAD:<path>` exports only the theme folder, with paths relative to it.
- `--prefix=wptv-27/` puts the files inside a `wptv-27/` folder, which is what WordPress expects when you upload a theme zip.
- `git archive` only exports **committed** files and reads `export-ignore` rules from the committed `.gitattributes`. Commit your changes before you build the zip.

To check what the zip will contain without creating it:

```bash
git archive --format=tar HEAD:wordpress.tv/public_html/wp-content/themes/wptv-27 | tar -t
```

## Proposing changes to the Meta repository

A pull request to [WordPress/wordpress.org](https://github.com/WordPress/wordpress.org) carries only the exported theme:

1. Merge `upstream/trunk` into `wptv-27-dev` first, so the export never undoes a change made in the Meta repository.
2. Create a branch from `upstream/trunk` and replace the theme folder with the export of `wptv-27-dev`.
3. Check that the only differences from `upstream/trunk` are the intended ones, push the branch to the fork and open the pull request.

## Language

All project content is written in **US English (en_US)**, following the WordPress standard: code, function names, comments, translatable strings, commit messages and documentation.

## License

GNU General Public License v2 or later.
