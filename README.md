# docs.webinertia.net

The published documentation site for the Webware PHP fleet, served at
<https://docs.webinertia.net/> from the `gh-pages` branch of this repository.

This repository holds configuration and automation only. It is not where the documentation
lives. Each onboarded project keeps its AsciiDoc sources in its own repository, under a `docs`
directory, and this site renders them all into one tree.

## The `gh-pages` branch is generated

Never edit `gh-pages` by hand, and never commit to it. It is an output branch owned entirely by
the publish workflow (`.github/workflows/publish-docs.yml`). Every publishing run replaces the
branch contents wholesale, so a manual commit there is silently destroyed on the next run.
Change the inputs instead: the project's AsciiDoc sources, or `site.json` in this repository.

## Onboarding a project

A repository is included when its `doc-target` repository custom property is `true`. Nothing else
selects a project; there is no list to maintain here. The same property drives the CI workflow
targeting, so a project onboarded once is onboarded everywhere.

## Operator guide

The full operator guide, covering the daily schedule, manual dispatch, the `CNAME` and
`.nojekyll` guards, the exit codes, and how to read a failed run, is the `docs/v1/operating.adoc`
document in [webinertia/asciidoc-php-multi-repo](https://github.com/webinertia/asciidoc-php-multi-repo).

## Contents of this repository

| Path | Purpose |
|---|---|
| `site.json` | Site configuration: organization, project selection property, custom hostname, output locations |
| `composer.json`, `composer.lock` | Pin the builder (`webware/asciidoc-php-multi-repo`) for a local build only; the workflow runs the container action |
| `.github/workflows/publish-docs.yml` | The scheduled, dispatchable publish pipeline |
| `README.md` | This file |

## Building locally

The publish workflow builds in the `webinertia/asciidoc-php-multi-repo` container action, so no
PHP toolchain is installed on the runner. The steps below are for a local preview only.

```sh
composer install --no-dev --no-interaction
php vendor/bin/asciidoc-php-multi-repo build --config site.json
```

Discovery reads the organization through the `DOC_DISCOVERY_TOKEN` environment variable, which
must hold a token with organization repository read access and access to repository custom
properties. Without that token a build cannot enumerate projects.

