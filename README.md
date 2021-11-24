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

This is a meant to be a pass/fail test which blocks pushing broken internal links, and optionally blocks pushing an update until any broken external link are fixed (external breakage can occur independently of repo changes and is not recommended). A reasonable use case for the external link check is to get notifications when a scheduled run (which you would create) of the action detects problem links.

### Inputs

| Input | Req'd | Default | Meaning |
|-------|-------|---------|---------|
| canonical-root | yes | _(nil)_ (will fail build unless specified in ``with``) | URLs beginning with this string are considered internal, in addition to site-relative URLs (e.g. beginning with slash ``/``) or page-relative URLs) |
| check-external | no | n/a | If "true" check all URLs including offsite (external) |
| download-site-as | yes | unminified-site | Name of artifact from build stage which contains the site (tarball, no compression) |
| output-directory | yes | public | Directory (in the artifact, above, containing the site) |

The tarball in the artifact pointed to by ``download-site-as`` must be named ``hugo-site.tar`` and contain the following:

* A subdirectory tree containing the site (default: _public_, optionally defined by ``output-directory``).
* ``package.json``
* (recommended) ``package-lock.json``

``package.json`` and ``package-lock.json`` are used by ``npm install`` to install ``hyperlink`` which does the link-checking.

``hyperlink`` is <https://www.npmjs.com/package/hyperlink/> with homepage <https://github.com/Munter/hyperlink>.

### Outputs

None

### Sample usage

```yaml
name: test-check-links
on:
  pull_request:
    types:
      - assigned
      - opened
      - synchronize
      - reopened
  push:
    branches:
      - 'feature**'
      - main
      - 'staging**'
    paths-ignore:
      - 'README.md'
      - 'LICENSE'
      - '.gitignore'
      - '.vscode'
      - 'scripts'
jobs:
  build-unminified-site:
    runs-on: ubuntu-20.04
    steps:
      - name: "Build Site with Hugo and Audit"
        uses: danielfdickinson/hugo-action-build-audit@main
        with:
          source-directory: src
          upload-npm-json: true
          upload-site-as: unminified-site
          use-lfs: false
  check-links:
    needs: build-unminified-site
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
      - name: Run hugo-action-check-links
        uses: danielfdickinson/hugo-action-check-links
        with:
          canonical-root: https://www.example.com/
```