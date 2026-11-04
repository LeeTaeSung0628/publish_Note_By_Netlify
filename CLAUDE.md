# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working in this repository.

## What this repository is

The **published static site** of the user's Obsidian vault (`public-Note`), served by Netlify at `https://lts.kr`. Every page is HTML produced by the Obsidian community plugin **webpage-html-export 1.9.0-3b** (theme `Things`, dark). There is no build step: Netlify serves the files as they are, and a push to the deployed branch is a deploy. `netlify.toml` 301-redirects unknown paths to the home page `🏠-taesung's-blog.html`; files at the repo root (including `README.md` and this file) are publicly reachable, so never put private information anywhere in this repo.

Most commits are Obsidian Git auto-commits (`vault backup: <timestamp>`), not hand-written history.

## Working with this user (user decisions 2026-11-03 — always apply)

- **5지선다 before every answer:** before answering any message — explanations and work requests alike — first ask the user multiple-choice questions with the AskUserQuestion tool (1–4 questions, **exactly 4 options each**, recommended option first and marked `(추천)`) to pin down what an accurate answer needs, and remind them that the tool's automatic "기타(직접 입력)" is the 5th choice (the tool allows at most 4 options). Read files/check state first so the options are concrete. Only exception: when the user's message is the answer to the questions you just asked, proceed with that answer without asking again (no question loops). The user writes in Korean; answer in Korean.
- **Three-repo workflow:** what the user studies in **`ATMS-Server`** is written up as notes in **`public-Note`** (new ones in its `New/` folder) and published **here** as HTML. **`ATMS-Server` is read-only in these sessions — never edit, commit or push to it.** Content must already be sanitized (no bank-internal IPs, hostnames, accounts or names — see `public-Note/CLAUDE.md`).
- **New pages must look exactly like the exported ones** (below). Notes from `public-Note/New/` go to `new/` here.

## Source → page mapping (`makeNamesWebStyle`)

- Path = source path with each folder and file name **lower-cased, whitespace → `-`, runs of `-` collapsed**; emoji, Korean, `&`, `()`, `_`, `+` are kept, `.md` → `.html`.
  - `project/ATMS/🏧 ATMS - CICD 구성.md` → `project/atms/🏧-atms-cicd-구성.html`
  - `project/New Project/☘ ANONI Chat - infra setup.md` → `project/new-project/☘-anoni-chat-infra-setup.html`
  - `사진 및 문서/Pasted image 20250520142253.png` → `사진-및-문서/pasted-image-20250520142253.png`
- `lib/metadata.json` → `sourceToTarget` holds the authoritative map for every exported file.

## Page format (copy an existing page's skeleton, e.g. `project/atms/*.html`)

- Minified HTML (the whole page is essentially one line). `<base href>` and `<meta id="root-path" root-path>` are the relative path to the root: `./` at the root, `..` one folder down, `../..` two folders down.
- `<head>`: `<title>` = note title (the H1), `description`/`og:*` meta (`Category - <title>`, `og:url` = `./https://lts.kr/<exportPath>`), `author` = `taesung's Blog`, RSS link, `<include src="lib/html/custom-head-content.html">`, `webpage.js` + MiniSearch 6.3.0 scripts, the `lib/styles/*.css` links, the inline sidebar `<style>` and `loadIncludes` script — identical on every page.
- `<body class="publish css-settings-manager theme-dark …">`: left sidebar (search box + `<include src="lib/html/file-tree.html">`), `div.document-container > div.markdown-preview-view > div.markdown-preview-sizer` with the rendered note, then the right sidebar "Table Of Contents" outline tree.
- Rendered note: `h1.page-title.inline-title` first; each heading becomes `div.el-hN.heading-wrapper > hN.heading + div.heading-children` and later content nests inside it; other blocks are wrapped in `div.el-p`, `div.el-hr`, `div.el-br`, `div.el-pre`, `div.el-ul`, `div.el-table`, `div.el-blockquote`, `div.el-div` …. Tags → `a.tag` to `?query=tag:<urlencoded>`; wikilinks → `a.internal-link` with `data-href="<note title>"` and `href="<exportPath>"`; callouts → `div.callout[data-callout=<type>]` with `.callout-title` (icon svg + `.callout-title-inner`) and `.callout-content`; code → `pre.language-<x> > code.language-<x>.is-loaded` with Prism `span.token` markup plus a `button.copy-code-button`; images → `<img alt="<file>" src="사진-및-문서/<web-name>">`. Inline HTML from the note (`<font color>`, `<u>`, `<br>`) is kept as is.

## Shared files to update when adding or changing a page

- `lib/html/file-tree.html` — left navigation tree (folder/file `tree-item` entries).
- `lib/metadata.json` — `webpages[<exportPath>]` (title, tags, headers, links, times, sourcePath…), `fileInfo`, `allFiles`, `shownInTree`, `sourceToTarget`.
- `lib/search-index.json` — serialized MiniSearch index (fields `title`, `aliases`, `headers`, `tags`, `path`, `content`).
- `lib/rss.xml` — RSS items.
- The HTML of every page that links the new note (category MOC page, home page "새로 추가된 글").

## Known quirks

- `deleteOldFiles` is off in the export settings, so pages of renamed/deleted notes linger (e.g. `lectur/🏫-강의.html`, `project/🔫-이슈·트러블슈팅.html`, the `project/new-project/🤑-…` pages). Leave them unless the user asks.
- `lib/html/custom-head-content.html` is the same file as `public-Note/custom-head-content.html` (dark theme forcing, floating buttons).
