# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Source for the [TeamCity Help](https://www.jetbrains.com/help/teamcity/) site, built with **JetBrains Writerside**. There are two published documentation instances that share the same `topics/` source:

- `tc` — TeamCity On-Premises (`tc.tree`, web path `/teamcity/2026.3/`)
- `tcc` — TeamCity Cloud (`tcc.tree`, web path `/teamcity/cloud/2026.3/`)

## Building

Build runs through the JetBrains Writerside IDE plugin or the Writerside build tool. The build configuration is in `cfg/build-script.xml`. There is no local CLI build command for this project — previews and validation happen inside the Writerside IDE plugin (IntelliJ-based).

Broken anchors, a missing images, dangling includes are all validation rules that are used when changes are committed and docs are built on a dedicated build farm. This introduces a gap between committing a broken markup and reading the failure report. To avoid that, verify these by hand with `grep` before considering an edit finished. See [Verify before asserting](#verify-before-asserting).

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

### Anchors

Section anchors are the **exact heading text with `+` replacing spaces**, and they are case-sensitive — not lowercase-hyphen slugs. A heading `## Revision Synchronization` is linked as `#Revision+Synchronization`.

To anchor something other than a heading, use the **`id` attribute**. This is the only correct way to create a target for a doc-to-doc reference. It attaches to any element — a paragraph, table, definition list, image, or `<def>` inside a deflist:

```markdown
This paragraph is the link target.
{id="custom-id"}

<def title="Setting name" id="setting-anchor">
```

Both forms are the same mechanism: `<def id="...">` is just `id` attached to a `<def>`.

Rules that matter in practice:

- **Do not create `<anchor name="..."/>` elements.** This legacy construct still appears throughout the repo, but it does not produce a working anchor. It is harmless where it sits, so leave existing ones alone unless something links to them — but never add a new one, and never "restore" one in place of an `id` attribute.
- If an anchor that something links to is still a legacy `<anchor name="..."/>`, converting it to `{id="..."}` on the element it describes is a genuine fix: the inbound links are currently dead.
- **Attach the `id` to the element the anchor is actually about.** Putting it on a neighbouring image or video lands readers somewhere they did not expect, even though the link technically resolves.
- **Before renaming a heading, find every inbound link to its old anchor** across all of `topics/` (and `redirection-rules.xml`), not just the file being edited. If nothing links to it, renaming is safe; if something does, update it in the same change.
- Changing heading text changes the anchor even when `help-id` stays the same. `help-id` keeps in-product help links working; it does not preserve doc-to-doc anchors.
- Repo convention is not the same as correctness. A pattern repeated hundreds of times can still be a dead construct; verify what actually works before defending the majority usage.

### Per-file conventions to match

Several conventions are not global — check the file you are editing and match it:

- **Heading capitalization.** Reference topics use Title Case (`## Build Features`). What's-new pages use sentence case (`## Pipeline enhancements`). Match the surrounding file; do not normalize across files.
- **Code fence language tags are capitalized**: ` ```Shell `, ` ```Kotlin `, ` ```XML `, ` ```JSON `, ` ```PowerShell `, ` ```Text `. YAML is the exception, overwhelmingly written lowercase (` ```yaml `). Count before choosing: `grep -rhoE '^```[A-Za-z]+' topics/*.md | sort | uniq -c`.
- **`<show-structure for="chapter" depth="2"/>`** at the top of a topic controls the page-structure panel. Subsections deeper than the declared depth will not appear there, which matters when adding `####` headings.
- **Labels** referenced as `<primary-label ref="..."/>` or `<secondary-label ref="..."/>` must be defined in `labels.list`. Verify the id exists before using it.

### Duplicated headings and list numbering

Two Writerside patterns look like mistakes but are deliberate:

- **A heading appearing twice in one file** is almost always a `tc`/`tcc` pair, where each variant carries its own instance filter. Do not "deduplicate" them. Be aware the two share one anchor, so a link to it is ambiguous by design.
- **Consecutive list items with the same number** do the same job for ordered lists: two items both numbered `1.` with `{instance="tc"}` and `{instance="tcc"}` render as a single item 1 in each instance, so the following `2.` is correct in both. Renumbering these breaks one instance.

### Snippets and includes

Reusable blocks are defined with `<snippet id="...">` and pulled in elsewhere:

```markdown
<include from="source-topic.md" element-id="snippet-id"/>
```

**A relative anchor inside a snippet resolves against the including file, not the defining one.** Writing `[text](#some-anchor)` inside a snippet breaks everywhere the snippet is included. Always fully qualify links inside snippets: `[text](defining-topic.md#some-anchor)`.

When editing a snippet, check who includes it (`grep -rn "element-id=\"<id>\"" topics/`) — the edit lands in every one of those files.

### Instance-specific content

When content differs between On-Premises and Cloud, add the instance filter immediately after the paragraph or element:

```markdown
This sentence applies only to on-premises.
{instance="tc"}

This one applies only to cloud.
{instance="tcc"}
```

Omitting `{instance=...}` means the content appears in both.

Two things to watch:

- **A link's instance scope must match its target's.** If a bullet links to a section marked `{instance="tc"}`, the bullet needs the same filter, or Cloud readers follow a link into content that does not render for them.
- When two instance variants of a paragraph differ by only one sentence, prefer shared text plus a single filtered sentence. Duplicated variants drift apart over time.

### Navigation trees

To make a new topic visible, add a `<toc-element>` entry to `tc.tree` and/or `tcc.tree`. Topics not in any tree are unreachable from the published site even if they compile.

Before linking to a topic, confirm it is in the tree for both instances if the link is unfiltered. Also note that unreferenced topics still sit in `topics/` and still get matched by repo-wide searches — see [Orphaned files](#orphaned-files).

### Variables

Reference variables defined in `v.list` as `%variable-name%`. The current product version variable is `product-version`, whose value is `2026.03` — note the zero-padded month. This differs from the `2026.3` used in the instance `web-path` and image paths in `writerside.cfg`, so do not copy a version string from a URL into prose.

### Images

Place images in `images/`. Reference them by filename only (no path prefix). Dark-mode variants follow the naming convention `filename_dark.png` and are automatically switched by Writerside.

- **Never reference an image without confirming it exists on disk.** `ls images/ | grep <name>`.
- Check for the `_dark.png` variant too; a missing dark variant is a real defect.
- Check whether a suitable image already exists before assuming a new one is needed.
- Images added to the repo but referenced nowhere are usually an oversight — either wire them in or remove them.
- When a screenshot shows UI text (labels, tooltips, inline help), read it: it is primary evidence about current behavior, and it frequently contradicts the prose around it.

### Historical and version-scoped content

Some files are an accurate record of what was true at a point in time and **must not be "corrected" to match current reality**:

- old release notes (`teamcity-*-release-notes.md`, `teamcity-release-notes-build-*.md`)
- archived what's-new pages (`what-s-new-in-teamcity-20*.md`, `what-s-new-in-teamcity-cloud.md`)
- version-anchored entries in `upgrade-notes.md` and `upgrade-notes-older-versions.md`

Only evergreen reference topics need current-state accuracy. Exclude the files above from any repo-wide terminology or UI-label sweep.

Conversely, when a change alters a default or tightens a behavior in a way that affects existing installations, an entry under the current version's heading in `upgrade-notes.md` is usually warranted — that page is where an administrator who skips the what's-new page will look.

### Redirects

When renaming or moving a topic, add a `<rule>` to `redirection-rules.xml` pointing old URLs to the new topic's `accepts-web-file-names-ref` or ID so existing external links don't break.

## Style rules (`stylechecks.xml`)

`stylechecks.xml` is the authoritative list — read it when in doubt. The entries below are a frequently-hit subset:

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

Beyond the checker:

- **Avoid marketing language and hype**, including on what's-new pages: "seamless", "empower", "massive update", "boasts", "as confident as ever". Accurate and human, not salesy. A claimed virtue ("we take a pragmatic approach") reads as marketing; a demonstrated one (naming what actually shipped) does not.
- Prefer plain verbs: "use" over "utilize", "connect" over "hook up".
- Prefer task-based numbered steps over narrative prose for "how to do X", and order steps the way the user performs them (a prerequisite property before the UI setting it unlocks, not after).
- **Reuse existing terminology instead of introducing a synonym.** Before picking a term, count what the docs already use (`grep -rho "pane\|panel" topics/*.md | sort | uniq -c`). If another section deliberately distinguishes two similar words, do not blur that distinction.
- Match UI labels and their casing exactly as the product shows them, including path separators (`Admin | Global Settings`), and wrap UI paths in bold.

## Reviewing and editing documentation

This is the part that distinguishes a useful review from a spell-check. Treat every review as two passes: fluency and concision first, then gap analysis.

### Establish what actually changed

Before reviewing, separate new content from pre-existing content, or you will report longstanding issues as if the change introduced them:

- `git status --short` and `git diff` for uncommitted work; `git show <commit>` for a named commit.
- When running a style or terminology check over a file, **check only the added lines** — `git diff -U0 | grep "^+"` — otherwise a long-lived file buries the real findings under decades of pre-existing violations. If a pre-existing issue is worth raising, say explicitly that it predates the change.
- **Re-read a file before editing it.** Files change between edits, including by hand while a review is in progress. Never assume your own last write is the current state; a stale assumption silently reverts someone else's work.

### Read beyond the file you were given

Reviewing a section properly almost always requires reading the documents around it: the related settings topic, the roadmap, the older topic that first defined a term, the authoritative topic for a setting mentioned in passing. Go read them. If one of them needs a small, well-justified fix as a result — a stale claim, a mismatched UI label, a broken cross-reference — make the edit and state plainly what was touched and why, especially outside the file originally pointed at.

### Check the surroundings for inconsistencies, including text nobody edited

New content routinely invalidates text that was correct before it landed. This is the highest-value thing a reviewer does, because the stale text is in a file nobody is looking at. Before calling a change done, search for:

- **Claims that assumed the old behavior.** When a feature flips a default or adds a restriction, `grep` repo-wide for the property name, the feature name, and every affected tool, page, or action. A note reading "X is unavailable unless Y is enabled" becomes false the moment Y becomes the default.
- **Preconditions that are now incomplete.** Sentences of the form "when the agent is connected, you can …" silently become wrong when a second condition is added. Search for the capability, not the feature name.
- **Reference doc vs what's-new drift.** These are written independently and drift apart. Compare them directly: a fact in one and not the other, or a what's-new entry that overstates the scope of what actually changed.
- **Stale roadmap, EAP, and "not yet supported" caveats** that the new feature just made obsolete. Removing a limitation entry is often the correct edit — a reference topic describes current state and has no "previously"; the changelog framing belongs on the what's-new page.
- **Terminology drift.** Count existing usage before introducing a variant, and check image filenames, which often encode the intended term.
- **Headings that no longer describe their content** after a subsection was added.
- <a id="orphaned-files"></a>**Orphaned files.** A topic absent from both `.tree` files does not publish but still holds stale text and duplicate `id=` values. Check with `grep -n "<topic-name>.md" tc.tree tcc.tree`. Flag these rather than quietly editing dead content; deleting a file is the user's call.
- **Literal placeholders**: `???`, `TODO`, `IMAGE`, `... description`, an unfilled build number. Always report them; never invent a value.

### Prioritize the user's perspective

For every new or rewritten section, ask explicitly: *if a reader with a real task reads this, what would they still have to ask?* Then look for:

- **Missing defaults.** What happens if the reader changes nothing? A section that explains how to choose between two modes but never says which one is active out of the box is incomplete.
- **Missing preconditions and consequences.** What has to be true first, and what happens as a result — including the failure case. "If I get redirected to pick a different target, are my original edits preserved or lost?" is the kind of question worth answering.
- **Ambiguity that changes outcomes.** Where two readings of a sentence produce materially different behavior, resolve it. "The aggregated duration of all jobs" reads as a sum; for parallel jobs the real figure may be wall-clock, and the difference decides whether a limit is exceeded.
- **"Why would I use this", not just "what does this do".** Where a setting's practical value is not obvious from its name, add a concrete scenario: "enable this so a cleanup or notification job still runs even if an earlier job fails" beats "this job will run regardless of upstream failure". Explain which option to pick and why, not only that options exist.
- **The cost or risk the reader is signing up for.** Licensing impact, billing, security exposure, resource consumption. If the product's own UI states it, the docs should too.

Calibration, so the review stays credible:

- **Do not flag what the UI already answers** via a tooltip, icon, or visible label once the reader is performing the task. Do flag behavioral and consequence questions the UI cannot show.
- **Before calling two sentences contradictory, verify they describe the same property.** State models often have independent axes — whether execution continued or was cancelled is not the same axis as what the final status becomes — and two claims can both be true.
- **Never invent a plausible-sounding answer to fill a gap.** Present findings as clear, prioritized questions. An explicit unanswered question is more useful than a confident guess, and a guess that is wrong is worse than a gap.
- **Separate "already handled" from "still open."** Do not list something as missing and then answer it in the same report; it wastes the reader's attention and obscures the real gaps.
- **If a behavior is reported as a possible bug or as likely to change soon, leave it vague.** Do not push to document unstable details; note them and move on.
- Distinguish a defect from a preference, and lead with the defects.

### Verify before asserting

Cheap `grep` checks that prevent the most common errors:

| Before you… | Check |
|---|---|
| Link to a section anchor | The heading exists with that exact text and casing |
| Rename a heading | Every inbound link to the old anchor, repo-wide |
| Reference an image | The file exists in `images/`, plus its `_dark.png` variant |
| Link to a topic | It is present in the relevant `.tree` file(s) |
| Add an unfiltered link | The target is not instance-filtered to the other instance |
| Link from inside a snippet | The anchor is fully qualified with the defining filename |
| State a default or property name | The authoritative topic for that setting, not memory |
| Introduce a term | How often each candidate already appears in `topics/` |
| Quote a UI label | A screenshot or the product, including exact casing |
| Claim a feature is new | Whether an earlier form of it already existed, and what specifically changed |

### Bulk replacements

Repo-wide terminology sweeps are high-risk. Do not run a blind find-and-replace.

1. **Enumerate and classify first.** List every candidate with surrounding context and sort it into change / skip / needs-context. Separating by case alone often resolves most of it — lowercase prose uses ("administration rights", "server administration best practices") are usually not UI references.
2. **Watch for the term inside a longer term.** Replacing `Administration` will also hit `Server Administration` and `Project Administration`, which may be different, still-correct UI elements. Protect compounds with a placeholder substitution, replace, then restore.
3. **Watch for third-party UI.** An integration topic may document another product's admin screens. The product name is often not on the same line — only the surrounding procedure heading reveals it. Enumerating the distinct page names behind the separator is an efficient way to spot the outliers.
4. **Exclude historical files** before starting.
5. **Dry-run, then review the diff for malformed output**, not just for the count of replacements. Check that every replaced UI path kept its bold/link formatting, and that no two spellings of the same label survive.
6. **Audit what remains** and account for every leftover occurrence as deliberate.

### Reporting

State plainly what was changed and why, grouped so the reasoning is checkable: the fix, the evidence for it, and the file it landed in. Call out edits made outside the file originally named. Keep genuinely open questions in a separate, prioritized list, and say explicitly when something was deliberately left alone and what it is waiting on.

## Branches

- `2026.3` is the main branch (PRs target here)
- Feature/version branches like `2026.3-build-chains` are used for in-progress work on specific versions or topics
