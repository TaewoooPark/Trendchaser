# Trendchaser

**A prism for the AI signal stream — disperses noise, converges what matters.**

<p align="center">
  <img src="https://img.shields.io/github/license/TaewoooPark/Trendchaser?style=flat-square&labelColor=000000&color=333333" alt="License">
  <img src="https://img.shields.io/github/stars/TaewoooPark/Trendchaser?style=flat-square&logo=github&logoColor=white&labelColor=000000&color=333333" alt="GitHub stars">
  <img src="https://img.shields.io/github/last-commit/TaewoooPark/Trendchaser?style=flat-square&labelColor=000000&color=333333" alt="Last commit">
  <img src="https://img.shields.io/badge/status-live-000000?style=flat-square&labelColor=000000&color=333333" alt="Status">
  &nbsp;
  <img src="https://img.shields.io/badge/Python_3.11+-000000?style=flat-square&logo=python&logoColor=white&labelColor=000000" alt="Python 3.11+">
  <img src="https://img.shields.io/badge/Codex_%2B_GitHub_Actions-000000?style=flat-square&logo=openai&logoColor=white&labelColor=000000" alt="Codex + GitHub Actions">
  <img src="https://img.shields.io/badge/feedparser-000000?style=flat-square&labelColor=000000" alt="feedparser">
  <img src="https://img.shields.io/badge/trafilatura-000000?style=flat-square&labelColor=000000" alt="trafilatura">
  <img src="https://img.shields.io/badge/requests-000000?style=flat-square&labelColor=000000" alt="requests">
  <img src="https://img.shields.io/badge/BeautifulSoup-000000?style=flat-square&labelColor=000000" alt="BeautifulSoup">
  <img src="https://img.shields.io/badge/lxml-000000?style=flat-square&labelColor=000000" alt="lxml">
  &nbsp;
  <img src="https://img.shields.io/badge/Telegram_HTML-000000?style=flat-square&logo=telegram&logoColor=white&labelColor=000000" alt="Telegram HTML">
  <img src="https://img.shields.io/badge/Notion_API-000000?style=flat-square&logo=notion&logoColor=white&labelColor=000000" alt="Notion API">
  <img src="https://img.shields.io/badge/KakaoTalk_relay-000000?style=flat-square&logo=kakaotalk&logoColor=FEE500&labelColor=000000" alt="KakaoTalk relay">
  <img src="https://img.shields.io/badge/Obsidian_sync-000000?style=flat-square&logo=obsidian&logoColor=white&labelColor=000000" alt="Obsidian sync">
  &nbsp;
  <img src="https://img.shields.io/badge/HuggingFace-000000?style=flat-square&logo=huggingface&logoColor=white&labelColor=000000" alt="HuggingFace">
  <img src="https://img.shields.io/badge/arXiv-000000?style=flat-square&logo=arxiv&logoColor=white&labelColor=000000" alt="arXiv">
  <img src="https://img.shields.io/badge/GitHub_Trending-000000?style=flat-square&logo=github&logoColor=white&labelColor=000000" alt="GitHub Trending">
  <img src="https://img.shields.io/badge/Hacker_News-000000?style=flat-square&logo=ycombinator&logoColor=white&labelColor=000000" alt="Hacker News">
  <img src="https://img.shields.io/badge/Semantic_Scholar-000000?style=flat-square&labelColor=000000" alt="Semantic Scholar">
  <img src="https://img.shields.io/badge/OpenAlex-000000?style=flat-square&labelColor=000000" alt="OpenAlex">
  <img src="https://img.shields.io/badge/RSS-000000?style=flat-square&logo=rss&logoColor=white&labelColor=000000" alt="RSS">
</p>

> *"Reading all of it is impossible. Reading none of it means missing the actual shifts."*

[한국어 README](./README.ko.md) &nbsp;·&nbsp; [**taewoopark.com** — author site](https://taewoopark.com)

<p align="center">
  <img src="./assets/logo.png" alt="Trendchaser logo — concentric points dispersing and converging like light through a prism." width="320">
</p>

---

> **This repository is a technical report for Trendchaser.**
> The actual live briefs are delivered to a public KakaoTalk open chat (anonymous join allowed):
> **→ https://open.kakao.com/o/pfQMgHsi**
>
> The implementation source is private. This repo documents the design, sources, and pipeline.

---

## Table of contents

1. [What you receive](#what-you-receive)
2. [Why a prism?](#why-a-prism)
3. [How it arrives](#how-it-arrives)
4. [Architecture](#architecture)
5. [Pipeline stages](#pipeline-stages)
6. [Source catalog](#source-catalog)
7. [Scoring formula](#scoring-formula)
8. [Tech stack](#tech-stack)
9. [Failure modes & resilience](#failure-modes--resilience)
10. [Output format](#output-format)
11. [Telemetry & validation](#telemetry--validation)
12. [Design principles](#design-principles)
13. [Releases](#releases)
14. [Author](#author)

---

## What you receive

Four short news briefs a day, sent straight to your phone. Each topic is a compact card: headline, three or four labeled `▸` bullets, and source links. The website archive keeps the expanded `DETAIL` block for deeper context.

```
<!-- TG-SPLIT -->

## [AI] Anthropic, 새 모델 SDK 공개

▸ 무엇: Anthropic이 자체 에이전트 SDK를 정식 공개했다.
▸ 새 점: 기존 API 위에서 도구 호출과 메모리를 한 단계 추상화했다.
▸ 의의: agent workflow를 직접 엮던 팀의 glue code를 줄인다.
▸ 다음 행동: 기존 tool-calling wrapper와 SDK 경계를 비교할 만하다.

🔗 자세히: https://taewoopark.com/trendchaser/...
🔗 원문: https://anthropic.com/news/...
```

You read it like a newspaper. You don't run anything.

---

## Why a prism?

A prism takes a single beam of white light, **disperses** it into the spectrum that was already inside, and a second prism **converges** the parts you actually need back into a usable beam.

Trendchaser is that second prism for your daily AI/dev feed.

```
   sources ──┐
   sources ──┤        ╱╲          ╲   ╱
   sources ──┼──────▶ ╱  ╲ ──────▶ ╲ ╱ ──▶  brief
   sources ──┤       ╱    ╲        ╳
   sources ──┘      ╱──────╲      ╱ ╲
                  disperse + score   converge
```

---

## How it arrives

Four slots a day, Korea time.

| Slot | When (KST) | Lookback window | Composition |
|---|---|---|---|
| **morning** | 10:00 | last 13h | up to 6 AI + 4 general |
| **afternoon** | 14:00 | last 5h | up to 4 AI + 3 general |
| **evening** | 18:00 | last 5h | up to 4 AI + 3 general |
| **night** | 22:00 | last 5h | up to 4 AI + 3 general |

Lookback windows are tuned to slot spacing so each story enters at most one brief. The quota is an upper bound, not padding pressure: if the fresh set is thin, the loop ships a shorter brief rather than filling with stale trending. Cumulative deduplication across 14 days of brief history kills repeats at source. Source failures are isolated — a 503 on one feed does not block the others.

---

## Architecture

PRISMA-Codex runs the whole pipeline at each slot through GitHub Actions plus Codex auth — server-side, with no machine of the operator's switched on. The current loop has six macro stages plus validation/export/publish gates: fetch, deduplicate, enrich + shortlist, score + editorial review, write + postcheck, publish + deliver.

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="./assets/architecture-dark.svg">
    <img alt="Trendchaser system architecture - 184 effective enabled channels, including the X follow-list counted per account, move through fetch, deduplicate, enrich, score, write, validate, publish, and deliver gates; state is persisted across main and claude/brief-stream, then sent to Telegram, KakaoTalk, Obsidian, the graph layer, and optional Notion archive." src="./assets/architecture.svg" width="100%">
  </picture>
</p>

| # | Stage | What it does |
|--:|---|---|
| 1 | **Fetch** | 184 effective enabled channels polled in parallel when the X following fan-out is counted per account; 101 enabled config entries; each source best-effort, failures isolated |
| 2 | **Deduplicate** | persistent 14-day state, cross-source batch dedup, brief-stream overlay, stray-branch overlay, semantic brief-history scan |
| 3 | **Enrich + Shortlist** | body extraction for the top 45 candidates, then a deterministic 3x shortlist with source, aggregator, and trending caps |
| 4 | **Score + Review** | 6-axis composite against `profile.md`, source-tier gates, stale-trending rules, hallucination/source verification loop |
| 5 | **Write + Validate** | split topic cards for Telegram, web-only expanded detail blocks, `TC-SCORES`, and postcheck for freshness/time/verification records |
| 6 | **Publish + Deliver** | `main` state update, ff-only `claude/brief-stream` publish, Obsidian/GraphRAG export, Telegram push → KakaoTalk relay, optional Notion archive |

Two-branch publish strategy: `state/seen.json` lives on `main` (the next run reads latest dedup state); briefs, raw snapshots, Obsidian notes, graph layers, and `briefs.json` accumulate on `claude/brief-stream` so Obsidian and the website can follow that branch.

---

## Pipeline stages

### 1. Fetch

Parallel collection across 20 enabled fetcher families (`rss`/Atom, `arxiv`, `semantic_scholar`, `openalex`, `paperswithcode`, `conference_schedule`, `proceedings_page`, `hn`, `hn_search`, `hf_papers`, `hf_models` [trending/new mode], `github_trending`, `github_search`, `sitemap`, `hf_posts`, `bluesky_search`, `mastodon`, `reddit_rss`, `vercel_x`, `vercel_watch`). Per-source `RateLimiter` (arxiv: 3.0 s, GitHub API: 1.0 s, default: 0.2 s). `ThreadPoolExecutor(max_workers=5)`. Each fetcher returns `list[Item]` and may not raise — failures bubble into a `failures[]` array on the orchestrator.

URL canonicalization via `canonicalize_url` (strip UTM/tracking params, lowercase host, normalize trailing slash). Title normalization via NFKD + lowercase + whitespace collapse. Item ID = `sha1(canonical_url)[:40]`.

### 2. Deduplicate

Three deduplication layers:

1. **Persistent dedup** — `state/seen.json` holds every item ID + `title_norm` from the last 14 days. Match on either kills the new item.
2. **Cross-source within-batch** — same `id` or `title_norm` across sources keeps only the highest-weighted one.
3. **Brief-history dedup** — Step 1 overlays `origin/claude/brief-stream` plus recent stray legacy `claude/*` branches, then Codex scans the last 14 days of `briefs/*.md` for URL/title overlap and semantic content-entity matches. URL/title match or ≥ 0.6 Jaccard overlap drops the item; same-event entity overlap (company + product + version/event) also drops it even when the URL differs.
4. **Cross-source entity confirmation** — DOI, arXiv ID, OpenReview ID, GitHub repo, Hugging Face model ID, model/release name, and normalized title fallback are collapsed into a `cross_confirm_key`. Items seen from two or more independent entities get `cross_confirm_count` / `cross_confirm_sources` metadata and a ranking boost, but never bypass freshness, source integrity, or hallucination gates.

### 3. Enrich

Pre-rank by `(effective source weight × normalized score) + 0.3 × normalized velocity`, take the top 45. Trending sources (`github_trending_*`, `hf_models_trending`) are multiplied by a **novelty factor** to penalize old repos riding short-term hype — `created_at`-based: ≤7d 1.3× / ≤30d 1.0× / ≤90d 0.7× / >90d 0.4×. For each item, fetch and extract body via `trafilatura.fetch_url + extract`, truncate to 1500 chars, store in `body_excerpt`. Fail-open: per-item try/except, failures leave `body_excerpt=""`.

`SKIP_TRAFILATURA_SOURCES = {arxiv_ai, youtube_ai, hf_papers, hf_models_trending, hf_models_new, github_trending_*}` — for these sources the upstream summary is more accurate than DOM extraction.

`ThreadPoolExecutor(max_workers=8)`. The 45-item cap exists because the later hallucination verification loop may drop a selected item and promote a replacement, so replacement candidates need body context too.

A **shortlist** stage then emits a 3x pool: morning AI 18 / general 12, other slots AI 12 / general 9. Deterministic rules force at least one Tier-S AI source when present, separately protect major repo releases, prefer General dev-primary sources over weak aggregators, and cap aggregators / Tier-D / trending feeds before the brief-writing step.

### 4. Score

Codex reads `profile.md` (curation profile) and computes a per-item composite. The current system is still a **6-axis score**, but the surrounding gates have become stricter since the 2026-05-11 loop update:

```
score = 0.25·signal + 0.25·affinity + 0.20·recency + 0.20·novelty + 0.05·velocity + 0.05·freshness
```

Component definitions:
- **signal** — `source_weight × normalized(source_score)`. Sources without numeric score use weight only (0.6 baseline).
- **affinity** — semantic match between item title + body excerpt and `profile.md` Priorities / AI Keywords / Boost.
- **recency** — hour-resolution decay: ≤3h → 100, ≤6h → 85, ≤12h → 65, ≤24h → 40, >24h → 15. For resurfacing feeds, recency uses `signal_at` for curated feeds and artifact age for trending feeds.
- **novelty** — "is this a true first-broadcast signal, or trending piggyback?" Distinguishes primary publishers from re-discovery feeds.
  - First-party broadcast (lab blog / release.atom / papers / firehose) → 100
  - Curators / aggregators → 65
  - Trending (existing artifacts re-surfacing) → `created_at`-graded (≤7d 80 / ≤30d 50 / ≤90d 25 / >90d 0)
- **velocity** — normalized item velocity (HN/HF score gain over time); 0.5 baseline if absent.
- **freshness** — default 100; any 14-day URL/title/entity overlap is intended to drop the item before final selection.

**Hard cutoffs** (drop regardless of score): `profile.md` `Mute` match · `published_at > 24h` (for resurfacing feeds, `signal_at > 24h`) · prior URL/title/entity match in the 14-day brief window · topic relevance gate for non-dev politics/lifestyle/non-IT industry drift. The old broad evergreen exception is gone; only Tier-S first-party items may relax to ≤72h when the slot would otherwise fall below the minimum topic floor.

### 5. Write

Codex selects per slot from the 3x pool:
- morning: up to 6 AI + 4 general
- afternoon, evening, night: up to 4 AI + 3 general
- all slots: minimum 3 topics if qualifying candidates exist; never pad with stale items just to fill quota

Before writing, Step 8.5 runs a **hallucination/source verification loop**. Load-bearing claims — affiliations, acronym expansions, numbers, model/repo identifiers, direct quotes — must be checked against the original URL when possible. If a claim fails, Codex strips/rephrases it or drops the item and promotes a replacement from the shortlist.

Each topic is composed as a Telegram card plus website detail: `## [AI]` / `## [General]` / optional `## [Watch]` heading, 3-4 `▸` bullets for the push message, `<!-- DETAIL -->` followed by 3-4 paragraphs of expanded context for the website, then `(출처: {source_id} — {bare_url} — {time})`. `<!-- TG-SPLIT -->` markers make each topic its own Telegram message. A footer records diagnostics plus `<!-- TC-SCORES {...} -->` for the website score meter.

### 6. Deliver

`md_to_telegram_html` via the placeholder→escape→restore pattern (allowed tags: `b`, `i`, `code`, `a`, `blockquote`). Markdown headings → bold lines, bullets → `• ` prefix. `split_brief_into_messages` first honors `<!-- TG-SPLIT -->`, then length-splits any oversized segment to ≤ 3800 chars.

For topic segments, the Telegram card keeps the bullets and rewrites `(출처: ...)` into two links: `자세히` deep-linking to `taewoopark.com/trendchaser`, and `원문` pointing at the collected source URL. The `<!-- DETAIL -->` expanded context and footer are kept in the brief file / website archive but stripped from Telegram.

Optional Notion archive: `notion_blocks.markdown_to_notion_blocks` converts to `heading_1..3`, `bulleted_list_item`, `numbered_list_item`, `quote`, `paragraph` (1900-char split), with inline `link/bold/italic/code` rich_text. Page properties: Title, Date, Slot, Sources (multi_select).

KakaoTalk relay is downstream of Telegram. The brief is also published to `claude/brief-stream` with Obsidian notes, graph clusters, typed relations, and `briefs.json`; `verify_main_brief.py` checks that both the canonical brief and its Obsidian export landed on the remote branch.

---

## Source catalog

As of the PRISMA-Codex expansion (2026-07-07), `sources.yaml` has **116 configured sources**, **101 enabled config entries**, and **184 effective enabled channels** when the private X follow-list is counted per account. The old Claude Routine surface has moved to GitHub Actions + Codex auth; the X/watch proxy remains separate so tokens, account names, and follow-list contents stay out of this public report. Disabled paths remain documented in config but are not counted here: retired feeds, Gmail newsletters, unstable YouTube RSS, low-signal X search, and experimental handles that are not part of the live loop.

| Category | Count | Role |
|---|---:|---|
| RSS / Atom firehose | **43** | first-party blogs, release atoms, curators, Korean dev blogs, longform, aggregators |
| X via Vercel proxy | **95 effective** | 84 private follow-list accounts plus 11 direct official / high-signal handles; account inventory is not public |
| Vercel watch | **17** | docs, leaderboards, conference pages, OpenReview venue pages without stable RSS |
| Scholarly APIs + conference/proceedings | **13** | arXiv, Semantic Scholar, OpenAlex, Papers With Code, ICML/NeurIPS/ICLR/MLSys schedules, ACL/CVPR proceedings |
| Hugging Face native surfaces | **4** | HF Papers, HF Models new/trending, HF community posts |
| GitHub discovery | **3** | GitHub Trending plus recent-push search for AI repos |
| Hacker News | **3** | top, breaking, and AI keyword search |
| Social/community smoke | **5** | Bluesky, Mastodon, Reddit MachineLearning, Reddit LocalLLaMA |
| Sitemap | **1** | Anthropic news sitemap |
| **Total effective enabled** | **184** | counts private X follow-list accounts individually while excluding their identities from the public report |

### PRISMA-Codex Expansion

The 2026-07-07 patch changed the collector from a handful of high-confidence feeds into a broader AI trend radar. The selection gate stayed conservative: weak sources are allowed to discover candidates, but promotion requires recency, source integrity, and either direct authority or cross-source confirmation.

| Layer | Newly emphasized sources | Weight range | Why it exists |
|---|---|---:|---|
| Tier S official / conference | `icml_2026_schedule`, `neurips_2026_schedule`, `iclr_2026_schedule`, `mlsys_2026_schedule`, official model docs watch | 1.35-1.75 | catches schedule, keynote, paper-list, and model-doc changes before they become summaries elsewhere |
| Scholarly indices | `semantic_scholar_latest_ai`, `openalex_latest_ai`, `paperswithcode_ai`, focused arXiv queries | 1.05-1.15 | broad paper coverage without trusting any one database as the final source of truth |
| Implementation/adoption smoke | `github_search_ai_repos`, GitHub Trending, HF Models/Posts | 0.9-1.4 | catches repos, weights, eval harnesses, and demos as soon as they start moving |
| Community early smoke | `reddit_machinelearning_new`, `reddit_localllama_new`, Bluesky, Mastodon, HN search | 0.6-1.3 | useful for discovery, intentionally low-authority unless confirmed elsewhere |
| Opaque-page watch | LMArena, Artificial Analysis, OpenRouter, COLM/CoRL/RSS/UAI/OpenReview pages | 1.0-1.35 | covers high-value pages that have no reliable RSS/API surface |

### Academic / Research Expansion

| ID | Type | Weight | Role |
|---|---|---:|---|
| `arxiv_llm_agents` | arXiv API | 1.15 | focused LLM/agent submissions, higher than raw all-AI arXiv because the query is narrower |
| `arxiv_multimodal_video` | arXiv API | 1.10 | multimodal/video generation and vision-language work |
| `arxiv_robotics_embodied` | arXiv API | 1.10 | robotics, embodied agents, and action-model papers |
| `semantic_scholar_latest_ai` | Semantic Scholar | 1.05 | cross-checks arXiv and catches indexed papers with metadata enrichment |
| `openalex_latest_ai` | OpenAlex | 1.05 | independent scholarly index; same layer as Semantic Scholar |
| `paperswithcode_ai` | Papers With Code | 1.15 | paper + implementation/adoption signal, above generic scholarly indices but below official/conference sources |
| `icml_2026_schedule` | Conference schedule | 1.75 | highest-weight academic event stream for the current ICML window |
| `neurips_2026_schedule` | Conference schedule | 1.70 | top ML conference schedule / program changes |
| `iclr_2026_schedule` | Conference schedule | 1.65 | top representation-learning conference surface |
| `mlsys_2026_schedule` | Conference schedule | 1.55 | systems-for-ML schedule surface |
| `acl_2026_proceedings` | Proceedings page | 1.20 | NLP proceedings discovery |
| `cvpr_2026_proceedings` | Proceedings page | 1.10 | vision proceedings discovery |

### Watch / Opaque Pages

| ID | Weight | Watches |
|---|---:|---|
| `watch_openai_models_docs` | 1.35 | OpenAI model docs diff |
| `watch_anthropic_models_docs` | 1.35 | Anthropic model docs diff |
| `watch_gemini_release_notes` | 1.25 | Gemini release notes |
| `watch_lmarena_leaderboard` | 1.25 | LMArena leaderboard movement |
| `watch_artificial_analysis_models` | 1.15 | Artificial Analysis model page |
| `watch_openrouter_models` | 1.00 | OpenRouter model catalog |
| `watch_colm_schedule` | 1.35 | COLM schedule |
| `watch_corl_home` | 1.20 | CoRL home / program surface |
| `watch_rss_accepted_papers` | 1.25 | RSS accepted papers page |
| `watch_uai_important_dates` | 1.10 | UAI important dates |
| `watch_openreview_colm_2026` / `watch_openreview_corl_2026` / `watch_openreview_rss_2026` / `watch_openreview_uai_2026` | 1.05 | OpenReview venue pages used as HTML-watch fallbacks when API access is unstable |


### AI Trend Primary

| ID | Type | Weight | Parameters |
|---|---|---|---|
| `hf_papers` | HuggingFace Daily Papers | **1.7** | 2-day lookback, min_upvotes=15 |
| `hf_models_new` | HuggingFace Models (firehose) | **1.5** | Foundation-lab author filter, `createdAt desc`, 5 per author, 7-day window. min_likes/downloads=0. |
| `github_trending_python` | GitHub Trending | 1.4 | language=python, since=daily, top 25, min_stars_today=30; evening/night 1.3x slot multiplier |
| `github_trending_overall` | GitHub Trending | 1.3 | all languages, since=daily, top 15, min_stars_today=100; evening/night 1.3x slot multiplier |
| `github_search_ai_repos` | GitHub Search | 1.1 | recent-push AI repository query; lower than Trending, higher than low-authority social smoke |
| `hf_models_trending` | HuggingFace Models | 0.9 | Cumulative-popularity signal; old-model freshness handled by detail fetch + novelty caps |

**Foundation lab whitelist**: Western, Chinese, and Korean lab orgs are hard-filtered for `hf_models_new`; authority-listed labs get lower thresholds in `hf_models_trending`.

### AI Lab Direct

| ID | Type | Weight | Feed |
|---|---|---|---|
| `anthropic_news` | Sitemap | **1.6** | anthropic.com/news via sitemap; morning 1.3x slot multiplier |
| `openai_blog` | RSS | 1.5 | openai.com/blog/rss.xml |
| `googleai_blog` | RSS | 1.3 | blog.google/technology/ai/rss/ |
| `deepmind_blog` | RSS | 1.3 | deepmind.google/blog/rss.xml |
| `huggingface_blog` | RSS | 1.3 | huggingface.co/blog/feed.xml |
| `watch_qwen_blog` | Vercel watch | 1.0 | qwen-blog changedetection alternative; afternoon 1.3x slot multiplier |
| `watch_upstage_blog` | Vercel watch | 1.0 | Upstage blog change probe |
| `watch_lg_research` | Vercel watch | 1.0 | LG AI Research change probe |
| ~~`meta_ai`~~ | RSS | — | **Disabled 2026-05-11.** `ai.meta.com/blog/rss/` returned 404 across all probed paths — Meta retired the feed. Replaced by `meta_llama_stack_releases` (see below). |
| ~~`mistral`~~ | RSS | — | **Disabled 2026-05-11.** `mistral.ai/news/feed.xml` returned 404 — Mistral retired the feed. Replaced by `mistral_client_python_releases` and `mistral_common_releases` (see below). |

### Curators & AI Newsletters

| ID | Type | Weight | Feed |
|---|---|---|---|
| `simonwillison` | RSS | 1.4 | simonwillison.net/atom/everything/ |
| `import_ai` | RSS | 1.4 | importai.substack.com/feed — Jack Clark weekly digest |
| `latent_space` | RSS | 1.3 | latent.space/feed |
| `interconnects` | RSS | 1.3 | interconnects.ai/feed |
| `smol_ai` | RSS | 1.2 | buttondown.email/ainews/rss |

### Open Source Releases (Atom firehose)

GitHub `releases.atom` updates within seconds of a tag push — effectively real-time and the earliest first-party broadcast surface for software releases. Major AI dev-tool releases get a second forced slot in the shortlist so they are not crowded out by a high-scoring lab blog.

| ID | Type | Weight | Feed |
|---|---|---|---|
| `claude_code_releases` | Atom | **1.7** | anthropics/claude-code |
| `openai_codex_releases` | Atom | **1.7** | openai/codex |
| `aider_releases` | Atom | 1.6 | Aider-AI/aider |
| `continue_releases` | Atom | 1.5 | continuedev/continue |
| `anthropic_sdk_releases` | Atom | 1.5 | anthropics/anthropic-sdk-python |
| `mcp_releases` | Atom | 1.5 | modelcontextprotocol/specification |
| `mcp_python_sdk_releases` | Atom | 1.6 | modelcontextprotocol/python-sdk |
| `mcp_typescript_sdk_releases` | Atom | 1.6 | modelcontextprotocol/typescript-sdk |
| `openai_python_releases` | Atom | 1.6 | openai/openai-python |
| `openai_agents_releases` | Atom | 1.6 | openai/openai-agents-python |
| `openai_swarm_releases` | Atom | 1.5 | openai/swarm |
| `transformers_releases` | Atom | 1.6 | huggingface/transformers |
| `accelerate_releases` | Atom | 1.5 | huggingface/accelerate |
| `diffusers_releases` | Atom | 1.5 | huggingface/diffusers |
| `peft_releases` | Atom | 1.5 | huggingface/peft |
| `vllm_releases` | Atom | 1.6 | vllm-project/vllm |
| `ollama_releases` | Atom | 1.6 | ollama/ollama |
| `llamacpp_releases` | Atom | 1.6 | ggml-org/llama.cpp |
| `langchain_releases` | Atom | 1.5 | langchain-ai/langchain |
| `autogen_releases` | Atom | 1.5 | microsoft/autogen |
| `meta_llama_stack_releases` | Atom | **1.5** | meta-llama/llama-stack — added 2026-05-11 to replace `meta_ai` RSS |
| `mistral_client_python_releases` | Atom | **1.5** | mistralai/client-python — added 2026-05-11 to replace `mistral` RSS |
| `mistral_common_releases` | Atom | **1.4** | mistralai/mistral-common — added 2026-05-11 to replace `mistral` RSS |

### Korean AI Ecosystem

| ID | Type | Weight | Feed |
|---|---|---|---|
| ~~`upstage_blog`~~ | RSS | — | Disabled after RSS probes 404; `watch_upstage_blog` and HF org signals cover it |
| ~~`lg_ai_research`~~ | RSS | — | Disabled after RSS probes 404; `watch_lg_research` and HF org signals cover it |
| `naver_d2` | Atom | 1.0 | d2.naver.com/d2.atom — CLOVA / HyperCLOVA; afternoon 1.3x |
| `kakao_tech` | RSS | 1.0 | tech.kakao.com/feed/ — Kakao Brain |

### Raw Scholarly Databases

| ID | Type | Weight | Parameters |
|---|---|---|---|
| `arxiv_ai` | arXiv API | 0.8 | categories=cs.AI/cs.LG/cs.CL/stat.ML, max_results=20, sort=submittedDate desc, rate_limit=3.0s |
| `arxiv_llm_agents` | arXiv API | 1.15 | focused LLM / agent query, 48h lookback |
| `arxiv_multimodal_video` | arXiv API | 1.1 | multimodal, VLM, video, generation query |
| `arxiv_robotics_embodied` | arXiv API | 1.1 | robotics / embodied agent query |
| `semantic_scholar_latest_ai` | Semantic Scholar | 1.05 | latest AI paper index, optional API key |
| `openalex_latest_ai` | OpenAlex | 1.05 | independent scholarly index; optional email/API key |
| `paperswithcode_ai` | Papers With Code | 1.15 | implementation-linked paper discovery |

### Aggregators

| ID | Type | Weight | Parameters |
|---|---|---|---|
| `hn_top` | Hacker News (Algolia, popularity) | 1.2 | tags=story, ai_min_points=120 / general_min_points=150, min_num_comments=20, lookback=30h |
| `hn_breaking` | Hacker News (Algolia, by date) | **1.4** | `/api/v1/search_by_date` firehose, 6h lookback, 75-point threshold for AI-keyword matches — catches early momentum before stories trend. |
| `techmeme` | RSS | 0.7 | techmeme.com/feed.xml (1.0 → 0.7, rumor + politics noise discount). 2026-05-11: aggregator carve-out — feed pubDate no longer presented as `published_at`, so 2-week-old essays re-surfaced by Techmeme stop being mis-tagged as "just published." |
| ~~`producthunt`~~ | RSS | — | **Disabled 2026-05-11.** Aggregator pubDate ≠ original publish time + low-signal launches. |

`hn_top` captures cumulative-popularity stories (already trending); `hn_breaking` captures early-momentum stories (becoming a story right now). The two channels are orthogonal on the time axis.

### Immediate Integration + Social

| ID | Type | Weight | Parameters |
|---|---|---|---|
| `hn_ai_filter` | HN Search | 1.3 | AI/dev keyword search, 12h lookback, min_points=30 |
| `lobsters_ai` | RSS | 1.2 | lobste.rs `ai,vibecoding`, 24h lookback |
| `hf_posts` | HF posts | 1.2 | 24h lookback, min_comments=5 |
| `devto_ai` / `devto_llm` / `devto_ml` | RSS | 0.5 | low-weight keyword feeds, max 8 items each |
| `bluesky_keyword_ai` | Bluesky Search | 1.1 | AI keyword discovery, 12h lookback |
| `reddit_machinelearning_new` | Reddit RSS | 0.6 | r/MachineLearning/new early smoke; keyword-filtered, low-authority unless confirmed |
| `reddit_localllama_new` | Reddit RSS | 0.6 | r/LocalLLaMA/new model/weights/quant smoke; capped and cross-confirm dependent |
| `mastodon_hashtag_aiml` / `mastodon_hashtag_fosstodon` | Mastodon | 0.9 / 0.7 | hashtag mode; favorite/reblog thresholds |

### X curation (limited)

The X surface has **95 effective channels**: 84 private `x_my_following` accounts plus 11 direct official lab / high-signal researcher handles. In `sources.yaml`, that appears as 12 enabled `vercel_x` config entries because `x_my_following` fans out privately. The official X API is **not** used — instead, a small self-hosted Vercel relay returns raw text only. Public logs keep success/failure status but never expose tokens, account inventory, or follow-list contents. `x_my_following` is the main surface, with a 24h lookback so the cron warmer's lag does not drop fresh posts. Channel rotation needs no code change.

### Longform & Essays

| ID | Type | Weight | Feed |
|---|---|---|---|
| `oneusefulthing` | RSS | 1.3 | oneusefulthing.org/feed — Ethan Mollick, applied AI essays |
| `pragmaticengineer` | RSS | 1.4 | newsletter.pragmaticengineer.com/feed — Gergely Orosz, software engineering longform |
| ~~`thebrowser`~~ | Gmail Newsletter | — | Disabled until Gmail connector auth is configured |
| ~~`notboring`~~ | Gmail Newsletter | — | Disabled until Gmail connector auth is configured |

### Meta-Source

| ID | Type | Weight | Feed |
|---|---|---|---|
| `lesswrong` | RSS | 1.1 | lesswrong.com/feed.xml?karmaThreshold=60, max=15 |
| `alignment_forum` | RSS | 1.0 | alignmentforum.org/feed.xml?karmaThreshold=20, max=10 |

### YouTube

| ID | Type | Weight | Channels |
|---|---|---|---|
| ~~`youtube_ai`~~ | YouTube RSS | — | Disabled after 2026-07-07 health probes found unstable 404/500 RSS endpoints. Channel pages can still be monitored manually or through future stable feeds. |

**AI classification rule** — source IDs in `AI_SOURCES` are AI by definition: HF papers/models/posts, arXiv focused queries, Semantic Scholar, OpenAlex, Papers With Code, lab blogs/watch paths, major release atoms, AI curators, alignment sources, GitHub Trending/Search, HN AI search, Reddit AI communities, conference/proceedings surfaces, and selected Korean AI signals. Other sources become AI only if title + body excerpt strongly match `profile.md` AI Keywords; otherwise they go through the General dev-domain gate.

---

## Scoring formula

Per-item composite score, computed by Codex at the scoring stage. The core score is a **6-axis composite**:

```
total = 0.25·signal       (source_weight × normalized source score)
      + 0.25·affinity     (semantic match against profile.md)
      + 0.20·recency      (hour-resolution decay step function)
      + 0.20·novelty      (first-broadcast vs trending piggyback)
      + 0.05·velocity     (normalized HN/HF velocity)
      + 0.05·freshness    (penalty for 14-day brief overlap)
```

Recency step function:

For resurfacing feeds, `published_at` is not always the true story time. Curated/index feeds (`hf_papers`, `hn_top`, `techmeme`, `lobsters_ai`, `semantic_scholar_latest_ai`, `openalex_latest_ai`, `paperswithcode_ai`) use `signal_at` for the 24h cutoff when available; trending/search feeds (`github_trending_*`, `github_search_ai_repos`, `hf_models_trending`) use artifact age or push time for recency scoring so an old repo that trends today does not read like a new launch.

```
hours since publish    score
        ≤ 3              100
        ≤ 6               85
        ≤ 12              65
        ≤ 24              40
        > 24              15
```

Novelty mapping:

```
source class                          score
First-party broadcast                  100
  (lab blog · release.atom ·
   hf_papers · conference schedule ·
   proceedings · hn_breaking · x_lab)
Official docs / leaderboard watch       85
Papers With Code / implementation index  65
Curator / aggregator                    65
Trending (artifact re-surfacing)     age-graded
  ≤7d                                   80
  ≤30d                                  50
  ≤90d                                  25
  >90d                                   0
Focused arXiv / scholarly index          60
Reddit / low-authority social smoke      35
```

This axis quantitatively separates "GitHub trending #1, but actually a 2-year-old repo riding a one-day hype" from "nvidia just pushed a release" — roughly trending 1.0× / first-broadcast 1.6×.

**Cross-source promotion**: DOI/arXiv/OpenReview/GitHub/HF/model identifiers and normalized-title fallback generate `cross_confirm_key`. If two or more independent entities report the same item, the candidate receives `cross_confirm_count` and `cross_confirm_sources` metadata plus a ranking boost. This is a promotion signal, not a bypass: freshness, original URL integrity, and hallucination verification still apply.

**Structural caps** (shortlist stage): the 3x pool caps aggregators, Tier-D sources, source repetition, and trending/search feeds. Trending/search sources (`github_trending_*`, `github_search_ai_repos`, `hf_models_trending`, Reddit, weak social search) are capped before final writing; final editorial review then prevents >7d stale-trending items from taking primary slots while fresher candidates remain.

Selection thresholds: AI items below 60 and General items below 50 are normally dropped. If the whole slot would fall below 3 topics, the loop relaxes in order: AI 55→50→45, Tier-S first-party items up to 72h, Watch promotion, then General down to 40 if it passes the dev-domain gate. If that still cannot reach 3 topics, the brief ships short and records the reason in diagnostics.

---

## Tech stack

| Layer | Choice | Why |
|---|---|---|
| Runtime | **Python 3.11+** with `from __future__ import annotations` everywhere | Type-hinted, dataclass-based items, available in GitHub Actions runners |
| Orchestrator | **GitHub Actions + Codex auth** | Cron-driven, fresh repo clone per run, native `git push` access, guarded KST slot execution |
| Parallelism | **`concurrent.futures.ThreadPoolExecutor`** (5 fetchers, 8 enrich workers) | I/O-bound — threads are sufficient, no asyncio complexity |
| RSS / Atom | **`feedparser` ≥ 6.0.10** | Tolerant of malformed feeds, normalizes Atom/RSS variants |
| HTML extraction | **`trafilatura` ≥ 1.12** + `BeautifulSoup4` + `lxml` | Highest-precision article body extraction in the open-source category |
| HTTP | **`requests` ≥ 2.31** with 20s timeout default | Per-call timeout, structured error handling |
| Time | **`python-dateutil` ≥ 2.8** | Robust ISO-8601 + RFC-822 parsing across feeds |
| Config | **`pyyaml` ≥ 6.0** for `sources.yaml` and `profile.md` | Human-editable, push-to-deploy |
| Scholarly APIs | arXiv, Semantic Scholar, OpenAlex, Papers With Code | Broad academic coverage; optional keys improve quotas but are not required for the baseline path |
| Delivery | **Telegram Bot API** (HTML mode, sendMessage) + **Notion API** (`notion-client` ≥ 2.2) | Telegram for push, Notion for optional archive — both free |
| Brief authoring | **Codex** — affinity scoring + prose composition | LLM does the curation judgement; deterministic Python wraps it |
| Storage | Two-branch git: `main` for `state/seen.json`, `claude/brief-stream` for `briefs/*.md`, `state/raw/*.json`, Obsidian notes, graph clusters, `briefs.json` | Race-safe dedup state + website/Obsidian-followable archive |
| Graph layer | deterministic Obsidian export + relation/cluster builders | No embeddings or paid vector DB; graph notes are regenerated from brief artifacts |
| KakaoTalk relay | Android-side relay forwarding Telegram messages | No official Kakao open-chat push API exists |

No vector DB. No external scoring service. The pipeline is intentionally cheap and small; Codex auth is used for the scheduled authoring run, while source/delivery credentials stay in repository secrets.

---

## Failure modes & resilience

| Failure | Behavior |
|---|---|
| Source 5xx / timeout | Fetcher returns `[]` + warning log. Orchestrator records `failures[source_id]`. Other sources continue. |
| arXiv rate limit (3s/req) | Per-source `RateLimiter` enforces inter-request delay. |
| HuggingFace API schema change | `hf_papers` URL normalization to canonical `arxiv.org/abs/...` so cross-source dedup with `arxiv_ai` still works. |
| Scholarly API quota / timeout | Semantic Scholar, OpenAlex, and Papers With Code fail open with source-level warnings; other academic sources continue. |
| Reddit 429 / blocked RSS | Reddit smoke sources return `[]` and stay capped; they are never required for a slot to ship. |
| OpenReview API challenge | Venue pages are watched through `vercel_watch` instead of assuming API access. |
| Hallucinated claim risk | Step 8.5 fetches original URLs for load-bearing claims; unverifiable claims are stripped, rephrased, or cause item replacement. |
| Stale topic leakage | `postcheck.py` rejects missing time signals and slot-window violations; Codex rewrites, replaces, or drops the topic before publish. |
| Telegram chunk send failure | Per-message try/except + 1 retry with backoff. Failed message does not block subsequent messages. |
| Notion API 5xx | Logged, brief is still delivered to Telegram. Notion archive is best-effort. |
| Gmail connector missing | Gmail newsletter sources are disabled in config; if re-enabled without connector auth, status is summarized as `gmail_status=disabled` rather than noisy per-slot failures. |
| YouTube RSS instability | `youtube_ai` is disabled until feed endpoints stabilize; no noisy per-slot failure. |
| Codex auth missing | The workflow checks for `CODEX_ACCESS_TOKEN` or `CODEX_AUTH_JSON` before the authoring step and fails loudly instead of silently sending a weak brief. |
| GitHub Actions early/late trigger | The workflow computes the KST slot, waits until the boundary when needed, and only publishes for 10:00 / 14:00 / 18:00 / 22:00 KST. |
| GitHub Actions repo race | `state/seen.json` pushes to `main`; `push_brief_stream.sh` builds a temp index on `origin/claude/brief-stream` and fast-forward pushes, retrying on concurrent updates. |
| Silent branch publish failure | `verify_main_brief.py` fetches the remote branch and confirms both `briefs/$DATE-$SLOT.md` and `obsidian/briefs/brief-$DATE-$SLOT.md` exist; missing files trigger a Telegram alert. |
| Single bad URL in enrich | Per-item try/except, leaves `body_excerpt=""`, scoring continues. |
| No env vars (TELEGRAM_*) | Delivery warns and exits 0 (does not crash the workflow). |

---

## Output format

Brief markdown structure (slot file at `briefs/$DATE-$SLOT.md`):

```markdown
# Trendchaser: 4월 30일 10시 기준 최신 AI/Dev 소식

<!-- TG-SPLIT -->

## [AI] 🚀 모델 SDK 공개

▸ 무엇: Anthropic이 새 모델 SDK를 공개.
▸ 새 점: 도구 호출과 메모리 관리를 한 단계 추상화.
▸ 의의: agent framework와 기존 API 사이의 간극을 줄임.

<!-- DETAIL -->

웹사이트에만 실리는 확장 해설 3-4문단. 배경, 핵심 내용, 작동 방식,
의의와 한계를 bullet 카드보다 길게 설명한다.

(출처: anthropic_news — https://www.anthropic.com/news/agent-sdk — 2시간 전)

<!-- TG-SPLIT -->

## [General] 런타임 릴리스

▸ 무엇: ...

<!-- TG-SPLIT -->

> 생성 2026-04-30 10:02 KST · 직전 슬롯 이후 13시간 스캔 · raw 201 → dedup 190 → 선정 7

---
*Failed sources: 없음*
*Diagnostics: top_signal=anthropic_news(2.43), oldest_picked=8h ago · hallu_fetched=8, hallu_fetch_failed=0*
<!-- TC-SCORES {"1":{"signal":90,"affinity":85,"recency":100,"novelty":100,"velocity":50,"freshness":100}} -->
```

Telegram receives one message per `<!-- TG-SPLIT -->` topic segment. The `<!-- DETAIL -->` section, diagnostics footer, and `TC-SCORES` comment are stripped from Telegram but remain in the brief file and website archive.

---

## Telemetry & validation

Current PRISMA-Codex validation gates:

| Test | Result |
|---|---|
| Fetch/dedupe/enrich/shortlist artifacts | `state/raw/$DATE-$SLOT.{json,dedup.json,enriched.json,shortlist.json}` |
| Hallucination loop record | footer must include `hallu_fetched=N, hallu_fetch_failed=M` |
| Postcheck | validates topic time signals, slot-window freshness, and hallucination-loop record |
| Publish check | confirms canonical brief + Obsidian export exist on `origin/claude/brief-stream` |
| Website bridge | `push_brief_stream.sh` rebuilds `briefs.json` for `taewoopark.com/trendchaser` |
| Delivery self-tests | cover HTML escaping, underscore URLs, split markers, chunk fallback, Notion blocks, env-missing exit 0 |

Live operation: PRISMA-Codex runs four times a day at 10:00 / 14:00 / 18:00 / 22:00 KST. Recent live briefs show the full card/detail format, `hallu_*` diagnostics, `TC-SCORES` footer used by the website, and a generation footer marked `by Codex`.

---

## Design principles

- **News-brief tone, not LinkedIn tone.** Compact topic cards plus grounded detail. No bullet salad, no "thought leadership," no empty adjectives like "groundbreaking."
- **Hour-resolution recency.** A story published 3 hours ago beats one from yesterday — even if yesterday's was technically "more relevant." Stale wins are not wins.
- **Cumulative deduplication.** A 14-day rolling window over every brief ever sent. Repeats die at source.
- **Source URL integrity.** Only URLs the fetcher actually collected. No synthesizing, no shortening, no replacing with search results. If verification fails, the item is dropped.
- **Failure isolation.** A 503 on HuggingFace does not take down the GitHub trending feed. Each source is best-effort; the pipeline ships what it has.
- **Push to deploy.** Edit `profile.md` or `sources.yaml`, push to `main`, the next slot picks up the change. No restart, no redeploy.

---

## Project status

Live operation. PRISMA-Codex runs four times a day at 10:00 / 14:00 / 18:00 / 22:00 KST and broadcasts to the KakaoTalk open chat linked above.

---

## Releases

User-facing change logs for every revision that affects what shows up in the brief:

| Date | Note | Headline |
|---|---|---|
| 2026-07-07 | [`releases/2026-07-07.md`](./releases/2026-07-07.md) · [한국어](./releases/2026-07-07.ko.md) | PRISMA-Codex migration - GitHub Actions + Codex auth, 184 effective enabled channels, academic/source expansion, cross-source confirmation, and stricter scheduled-slot guards |
| 2026-06-12 | [`releases/2026-06-12.md`](./releases/2026-06-12.md) · [한국어](./releases/2026-06-12.ko.md) | Current PRISMA loop documented - 67 enabled channels, X follow-list fan-out, topic-card briefs, hallucination verification, GraphRAG export, and ff-only `brief-stream` publishing |
| 2026-05-11 | [`releases/2026-05-11.md`](./releases/2026-05-11.md) · [한국어](./releases/2026-05-11.ko.md) | Freshness gates land — old articles stop being tagged as "just published," dead Meta/Mistral RSS replaced with live release atoms, X channels start flowing again |
| 2026-05-04 | [`releases/2026-05-04.md`](./releases/2026-05-04.md) · [한국어](./releases/2026-05-04.ko.md) | Release-firehose expansion + novelty axis — first-party signal pushed to the top of the brief |

---

## Author

Built by **Taewoo Park** — physics + math @ KAIST, research intern at the KAIST Ultrafast Spin Dynamics Lab. Aligning physics, code, and culture toward a civilization-scale solution.

<p align="center">
  <a href="https://taewoopark.com"><img src="https://img.shields.io/badge/Website-taewoopark.com-000000?style=flat-square&logo=safari&logoColor=white&labelColor=000000" alt="Website"></a>
  <a href="https://github.com/TaewoooPark"><img src="https://img.shields.io/badge/GitHub-TaewoooPark-000000?style=flat-square&logo=github&logoColor=white&labelColor=000000" alt="GitHub"></a>
  <a href="https://x.com/theoverstrcture"><img src="https://img.shields.io/badge/X-@theoverstrcture-000000?style=flat-square&logo=x&logoColor=white&labelColor=000000" alt="X"></a>
  <a href="https://www.linkedin.com/in/taewoo-park-427a05352"><img src="https://img.shields.io/badge/LinkedIn-Taewoo%20Park-000000?style=flat-square&logo=linkedin&logoColor=white&labelColor=000000" alt="LinkedIn"></a>
  <a href="https://www.instagram.com/t.wo0_x/"><img src="https://img.shields.io/badge/Instagram-@t.wo0__x-000000?style=flat-square&logo=instagram&logoColor=white&labelColor=000000" alt="Instagram"></a>
  <a href="https://www.instagram.com/hustlyarchiv.kr/"><img src="https://img.shields.io/badge/Archive-@hustlyarchiv.kr-000000?style=flat-square&logo=instagram&logoColor=white&labelColor=000000" alt="Instagram archive"></a>
  <a href="mailto:ptw151125@kaist.ac.kr"><img src="https://img.shields.io/badge/Email-ptw151125@kaist.ac.kr-000000?style=flat-square&logo=gmail&logoColor=white&labelColor=000000" alt="Email"></a>
  <a href="https://open.kakao.com/o/pfQMgHsi"><img src="https://img.shields.io/badge/Trendchaser_Open_Chat-000000?style=flat-square&logo=kakaotalk&logoColor=FEE500&labelColor=000000" alt="Trendchaser Open Chat"></a>
</p>

Other things I'm building:
[NODEPROMPT](https://github.com/TaewoooPark/NODEPROMPT) &nbsp;·&nbsp; [PAIDEIA](https://github.com/TaewoooPark/PAIDEIA) &nbsp;·&nbsp; [PAIDEIA-codex](https://github.com/TaewoooPark/PAIDEIA-codex) &nbsp;·&nbsp; [taewoopark.com](https://github.com/TaewoooPark/taewoopark.com)

---

<p align="center">
  <sub>Trendchaser — disperse, score, converge.</sub>
</p>
