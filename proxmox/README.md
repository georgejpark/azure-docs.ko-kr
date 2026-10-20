# azure-docs.ko-kr on Proxmox

| | |
|---|---|
| Upstream | George's own repository (fork of MicrosoftDocs/azure-docs.ko-kr, default branch `live`, last commit 2018-12-26; 5 commits by MunChan Park in Nov–Dec 2018; no LICENSE file in the tree — content is Microsoft's Korean-localized Azure documentation) |
| Kind | docs |
| Cloud verdict | NOTHING-TO-DEPLOY |
| GPU | none |
| Target host | none (optional static preview on pve5) |
| Effort | S |

## What it is
A 2018 snapshot of the Korean translation of Azure documentation: 7,078 Markdown articles under `articles/` (170 service folders), `includes/`, `bread/toc.yml`, `portal-articles/`, and the Microsoft DocFX/OPS build configuration (`docfx.json`, `.openpublishing.publish.config.json`, `.openpublishing.redirection.json`). It is content, not software; there is no application to run. The repo weighs 2.0 GB including `.git` (11,576 tracked files, 13,544 total).

## Cloud dependency evidence
| File:line | What | Needed for core use? |
|---|---|---|
| `.openpublishing.build.ps1:2` | downloads `.openpublishing.buildcore.ps1` from `https://opbuildstorageprod.blob.core.windows.net/...` (Azure Blob) — Microsoft's private OPS build | No — only for Microsoft's docs.microsoft.com pipeline |
| `.openpublishing.publish.config.json` `dependent_repositories` | `_themes` ← `Microsoft/templates.docs.msft.ko-kr`, `_themes.pdf`, `_repo.en-us` | No — rendering templates for the official site; the Markdown reads fine without them |
| `docfx.json` `globalMetadata.feedback_github_repo` | points at `MicrosoftDocs/azure-docs.ko-kr` | No |
| article front matter (e.g. `articles/cortana-analytics-architecture-demand-forecasting-energy.md:1-20`) | `ms.openlocfilehash`, `ms.sourcegitcommit`, `ms.translationtype: HT` | No — localization metadata |

## Runtime requirements
None to read the content. Optional rendering: DocFX (.NET tool) can build `docfx.json`, but the `_themes` template repo is a Microsoft dependency that is not in this clone. No databases, ports, env vars, secrets or external APIs. Git: default branch is `live` (no `main`); `origin` = `https://github.com/georgejpark/azure-docs.ko-kr.git`.

## Proxmox plan
Nothing to deploy. If a browsable copy on the homelab is wanted, two cheap options (both unverified against upstream docs, which do not cover self-hosting):

**Option 1 — raw Markdown over HTTP**: LXC on **pve5**, Debian 13, 1 vCPU, 512 MB RAM, 8 GB disk; `apt-get install -y nginx git`, clone into `/var/www/azure-docs`, serve as static files; read articles with any Markdown viewer.
**Option 2 — DocFX static site**: same LXC with 2 GB RAM; install the .NET SDK and `dotnet tool install -g docfx` (to verify on Debian 13), run `docfx build docfx.json -o /var/www/azure-docs/_site` and serve `_site` with nginx. Expect warnings about the missing `_themes`; the default DocFX template is used instead.

Substitutes for the Microsoft pipeline: none needed — `.openpublishing.build.ps1` is not required to view or build the Markdown.

## Cloud later
Not applicable. The only cloud component (OPS build on Azure Blob) belongs to Microsoft, not to this repo.

## Test plan
- `find articles -name '*.md' | wc -l` returns 7078 (count measured at snapshot time; 7,078 Markdown files across the tree).
- Option 1: `curl -sI http://<ct-ip>/articles/azure-glossary-cloud-terminology.md` returns `200`.
- Option 2: `docfx build docfx.json` exits 0 (warnings allowed) and `http://<ct-ip>/` shows the rendered TOC from `bread/toc.yml`.

## Open questions
- Whether to keep this repository at all: it is a stale 2018 localization snapshot with five small contributions by George; for a portfolio it may be better archived or reduced to the commits he authored.
- The repo has no `main` branch; this plan was committed on `proxmox/plan` branched from `live`.
- Licensing for republishing Microsoft's localized docs on a public host (the upstream MicrosoftDocs repos carry CC BY 4.0 for docs, but no LICENSE file is present here) — fine on the private LAN, check before any public hosting.
