# hugo-action-check-links
GitHub Action to check links (internal-only or optionally all links) for a Hugo site

## Status

### Main

![test-build-validate](https://github.com/danielfdickinson/hugo-action-check-links/actions/workflows/test-check-links.yml/badge.svg)

### Pull Request

![test-build-validate PR](https://github.com/danielfdickinson/hugo-action-check-links/actions/workflows/test-check-links.yml/badge.svg?event=pull_request)

## Details

**NB**: Currently experimental and not for public consumption. Use at your own risk.

### Purpose

This is a meant to be a pass/fail test which blocks committing broken links.
### Inputs

| Input | Req'd | Default | Meaning |
|-------|-------|---------|---------|
| canonical-root | yes | _(nil)_ (will fail build unless specified in ``with``) | URLs beginning with this string are considered internal, in addition to site-relative URLs (e.g. beginning with slash ``/``) or page-relative URLs) |
| check-external | no | n/a | If "true" check all URLs including offsite (external) |
| download-site-as | yes | unminified-site | Name of artifact from build stage which contains the site (tarball, no compression) |
| output-directory | yes | public | Directory (in the artifact, above, containing the site) |

### Outputs

None

