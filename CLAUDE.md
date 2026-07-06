# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Source for the [TeamCity Help](https://www.jetbrains.com/help/teamcity/) site, built with **JetBrains Writerside**. There are two published documentation instances that share the same `topics/` source:

- `tc` — TeamCity On-Premises (`tc.tree`, web path `/teamcity/2026.2/`)
- `tcc` — TeamCity Cloud (`tcc.tree`, web path `/teamcity/cloud/2026.2/`)

## Building

Build runs through the JetBrains Writerside IDE plugin or the Writerside build tool. The build configuration is in `cfg/build-script.xml`. There is no local CLI build command for this project — previews and validation happen inside the Writerside IDE plugin (IntelliJ-based).

## Project structure

| Path | Purpose |
|---|---|
| `topics/` | All documentation source files (`.md` and `.topic`) |
| `images/` | Screenshots and static images referenced by topics |
| `media/` | Videos and animated assets |
| `tc.tree` | TOC/navigation for TeamCity On-Premises |
| `tcc.tree` | TOC/navigation for TeamCity Cloud |
| `writerside.cfg` | Writerside project root config (instances, vars, dirs) |
| `v.list` | Reusable text variables (e.g. `product-version`) |
| `c.list` | Category definitions |
| `g.list` | Tag definitions |
| `labels.list` | Label definitions |
| `stylechecks.xml` | Enforced terminology rules |
| `redirection-rules.xml` | URL redirect mappings for renamed/moved pages |
| `cfg/` | Build scripts and auxiliary config |

## Authoring topics

### File format

Topics are Markdown files with Writerside extensions. Every topic must start with:

```markdown
[//]: # (title: Page Title)
[//]: # (help-id: Optional Help ID)
```

Cross-references use the bare topic filename (no path, no extension needed for the title):

```markdown
See [](other-topic.md) for details.
[Custom link text](other-topic.md)
```

### Instance-specific content

When content differs between On-Premises and Cloud, add the instance filter immediately after the paragraph or element:

```markdown
This sentence applies only to on-premises.
{instance="tc"}

This one applies only to cloud.
{instance="tcc"}
```

Omitting `{instance=...}` means the content appears in both.

### Navigation trees

To make a new topic visible, add a `<toc-element>` entry to `tc.tree` and/or `tcc.tree`. Topics not in any tree are unreachable from the published site even if they compile.

### Variables

Reference variables defined in `v.list` as `%variable-name%`. The current product version variable is `product-version` (currently `2025.11`).

### Images

Place images in `images/`. Reference them by filename only (no path prefix). Dark-mode variants follow the naming convention `filename_dark.png` and are automatically switched by Writerside.

### Redirects

When renaming or moving a topic, add a `<rule>` to `redirection-rules.xml` pointing old URLs to the new topic's `accepts-web-file-names-ref` or ID so existing external links don't break.

## Style rules (`stylechecks.xml`)

The style checker enforces these (selected important ones):

- **click** not "click on"
- **checkbox** not "check-box" or "check box"
- **dialog** not "dialog box" or "dialogue"
- **for example** not "e.g."
- **that is** not "i.e."
- **and so on** not "etc."
- **plugin** not "plug-in"
- **popup** not "pop-up"
- **list** not "drop-down list" or "list box"
- **option** not "radio button"
- **field** not "text box"
- **filename** not "file name" or "file-name"
- Avoid: "please", "simply", "a.k.a", "in question"

## Branches

- `2026.2` is the main branch (PRs target here)
- Feature/version branches like `2026.2-build-chains` are used for in-progress work on specific versions or topics
