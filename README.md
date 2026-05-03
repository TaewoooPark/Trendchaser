# Trendchaser

**A prism for the AI signal stream — disperses noise, converges what matters.**

<p align="center">
  <img src="https://img.shields.io/github/license/TaewoooPark/Trendchaser?style=flat-square&labelColor=000000&color=333333" alt="License">
  <img src="https://img.shields.io/github/stars/TaewoooPark/Trendchaser?style=flat-square&logo=github&logoColor=white&labelColor=000000&color=333333" alt="GitHub stars">
  <img src="https://img.shields.io/github/last-commit/TaewoooPark/Trendchaser?style=flat-square&labelColor=000000&color=333333" alt="Last commit">
  <img src="https://img.shields.io/badge/status-live-000000?style=flat-square&labelColor=000000&color=333333" alt="Status">
  &nbsp;
  <img src="https://img.shields.io/badge/Python_3.11+-000000?style=flat-square&logo=python&logoColor=white&labelColor=000000" alt="Python 3.11+">
  <img src="https://img.shields.io/badge/Claude_Code_Routines-000000?style=flat-square&logo=anthropic&logoColor=white&labelColor=000000" alt="Claude Code Routines">
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
  <img src="https://img.shields.io/badge/YouTube-000000?style=flat-square&logo=youtube&logoColor=white&labelColor=000000" alt="YouTube">
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
6. [Source catalog (25 feeds)](#source-catalog)
7. [Scoring formula](#scoring-formula)
8. [Tech stack](#tech-stack)
9. [Failure modes & resilience](#failure-modes--resilience)
10. [Output format](#output-format)
11. [Telemetry & validation](#telemetry--validation)
12. [Design principles](#design-principles)
13. [Author](#author)

---

## What you receive

Three short news briefs a day, sent straight to your phone. Each item is a tight read — bold headline, three to five sentences of plain context, a source link.

```
🤖 AI

Anthropic, 새 모델 SDK 공개

Anthropic이 자체 에이전트 SDK를 정식 공개했다. 기존 API
위에서 도구 호출과 메모리를 한 단계 추상화한 것으로, ...
(출처: anthropic_news — https://anthropic.com/news/... — 2시간 전)

🌐 General

...
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
| **morning** | 10:00 | last 13h | 5 AI + 3 general |
| **afternoon** | 14:00 | last 5h | 3 AI + 2 general |
| **evening** | 18:00 | last 5h | 3 AI + 2 general |
| **night** | 22:00 | last 5h | 3 AI + 2 general |

Lookback windows are tuned to slot spacing so each story enters at most one brief. Cumulative deduplication across 14 days of brief history kills repeats at source. Source failures are isolated — a 503 on one feed does not block the others.

---

## Architecture

```
                ┌──────────────────────────────────────────────────────────────┐
                │                    Claude Code Routine (cloud)                │
                │  triggered by cron at 10:00 / 14:00 / 18:00 / 22:00 KST               │
                └──────────────────────────────────────────────────────────────┘
                                          │
                                          ▼
   ┌────────────────────────────────────────────────────────────────────────┐
   │                       12-step curation routine                         │
   │   driven by prompts/curate.md — Claude executes step-by-step           │
   └────────────────────────────────────────────────────────────────────────┘
        │           │             │             │             │
        ▼           ▼             ▼             ▼             ▼
   ┌────────┐  ┌─────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
   │ fetch_ │→ │ dedupe  │→ │ enrich   │→ │ Claude   │→ │ deliver  │
   │ all.py │  │ .py     │  │ .py      │  │ scoring  │  │ .py      │
   │        │  │         │  │          │  │ + write  │  │          │
   │ 9 fet- │  │ seen    │  │ trafila- │  │ (Step 6– │  │ Telegram │
   │ chers  │  │ .json   │  │ tura top │  │ 8 of     │  │ + Notion │
   │ paral- │  │ + within│  │ 30 body  │  │ curate)  │  │          │
   │ lel    │  │ batch   │  │ extract  │  │          │  │          │
   └────────┘  └─────────┘  └──────────┘  └──────────┘  └──────────┘
        │           │             │             │             │
        ▼           ▼             ▼             ▼             ▼
   raw items    deduplicated   enriched      brief md     Telegram chat
                items          items                      → KakaoTalk relay
                                                          → Notion archive
                                                          → Obsidian vault
                                                            (auto-pull
                                                             claude/brief-stream)
```

Two-branch publish strategy: `state/seen.json` lives on `main` (next routine reads latest dedup state); briefs accumulate on `claude/brief-stream` so Obsidian follows that branch.

---

## Pipeline stages

### 1. Fetch — `scripts/fetch_all.py`

Parallel collection across 10+ fetcher types (`rss`, `atom`, `arxiv`, `hn`, `youtube`, `hf_papers`, `hf_models` [trending/new mode], `github_trending`, `sitemap`, `gmail_newsletter`, `vercel_x`). Per-source `RateLimiter` (arxiv: 3.0 s, default: 0.2 s). `ThreadPoolExecutor(max_workers=5)`. Each fetcher returns `list[Item]` and may not raise — failures bubble into a `failures[]` array on the orchestrator.

URL canonicalization via `canonicalize_url` (strip UTM/tracking params, lowercase host, normalize trailing slash). Title normalization via NFKD + lowercase + whitespace collapse. Item ID = `sha1(canonical_url)[:40]`.

### 2. Deduplicate — `scripts/dedupe.py`

Three deduplication layers:

1. **Persistent dedup** — `state/seen.json` holds every item ID + `title_norm` from the last 14 days. Match on either kills the new item.
2. **Cross-source within-batch** — same `id` or `title_norm` across sources keeps only the highest-weighted one.
3. **Brief-history dedup** — Claude scans the last 14 days of `briefs/*.md` for `title_norm` substring or URL canonical match. ≥ 0.6 Jaccard overlap → freshness penalty −100 (effective drop).

### 3. Enrich — `scripts/enrich.py`

Pre-rank by `(weight × normalized score) + 0.3 × normalized velocity`, take top 30. Trending sources (`github_trending_*`, `hf_models_trending`) are multiplied by a **novelty factor** to penalize old repos riding short-term hype — `created_at`-based: ≤7d 1.3× / ≤30d 1.0× / ≤90d 0.7× / >90d 0.4×. For each item, fetch and extract body via `trafilatura.fetch_url + extract`, truncate to 1500 chars, store in `body_excerpt`. Fail-open: per-item try/except, failures leave `body_excerpt=""`.

`SKIP_TRAFILATURA_SOURCES = {arxiv_ai, youtube_ai, hf_papers, hf_models_trending, hf_models_new, github_trending_*}` — for these sources the upstream summary is more accurate than DOM extraction.

`ThreadPoolExecutor(max_workers=8)`. Empirical wall-time ≈ 3 s for top-30.

A **shortlist** stage (`scripts/shortlist.py`) then enforces a hard cap on trending sources: max 2 trending items in the AI section, max 1 in General. This blocks "old trending dominating the brief" structurally, before the brief-writing step.

### 4. Score — `prompts/curate.md` Step 6

Claude reads `profile.md` (curation profile) and computes a per-item composite. As of 2026-05-04, this is a **6-axis system with a novelty axis added**:

```
score = 0.25·signal + 0.25·affinity + 0.20·recency + 0.20·novelty + 0.05·velocity + 0.05·freshness
```

Component definitions:
- **signal** — `source_weight × normalized(source_score)`. Sources without numeric score use weight only (0.6 baseline).
- **affinity** — semantic match between item title + body excerpt and `profile.md` Priorities / AI Keywords / Boost.
- **recency** — hour-resolution decay: ≤3h → 100, ≤6h → 85, ≤12h → 65, ≤24h → 40, >24h → 15.
- **novelty** — "is this a true first-broadcast signal, or trending piggyback?" Distinguishes primary publishers from re-discovery feeds.
  - First-party broadcast (lab blog / release.atom / papers / firehose) → 100
  - Curators / aggregators → 65
  - Trending (existing artifacts re-surfacing) → `created_at`-graded (≤7d 80 / ≤30d 50 / ≤90d 25 / >90d 0)
- **velocity** — normalized item velocity (HN/HF score gain over time); 0.5 baseline if absent.
- **freshness** — penalty for items overlapping the last 14 days of briefs.

**Hard cutoffs** (drop regardless of score): `profile.md` `Mute` match · `published_at > 24h` (unless `signal+affinity` average ≥ 80, in which case marked "어제 등장") · prior URL match in 14-day brief window.

### 5. Write — `prompts/curate.md` Step 7–8

Claude selects per slot (with source-diversity guard, max 2 items per `source_id` per section):
- morning: 5 AI + 3 general
- afternoon, evening: 3 AI + 2 general

Each item is composed as a **2-block structure**: bold one-line headline (8–18 chars, news-headline tone) → blank line → 3–5 sentence prose paragraph closing with `(출처: {source_id} — {bare_url} — {N시간 전})`. Bare URL is mandatory — Telegram→KakaoTalk relay drops hyperlinks but preserves URL strings.

### 6. Deliver — `scripts/deliver.py`

`md_to_telegram_html` via the placeholder→escape→restore pattern (allowed tags: `b`, `i`, `code`, `a`, `blockquote`). Markdown headings → bold lines, bullets → `• ` prefix. Output is split into ≤ 3800 char chunks (paragraph-first, then line-level, then hard-split).

`strip_outbound_footer` removes `## 📌 다음 슬롯…` preview, `---`, `*Failed sources*`, `*Diagnostics*` from the outbound message — the brief file and Notion archive retain these for history.

Optional Notion archive: `notion_blocks.markdown_to_notion_blocks` converts to `heading_1..3`, `bulleted_list_item`, `numbered_list_item`, `quote`, `paragraph` (1900-char split), with inline `link/bold/italic/code` rich_text. Page properties: Title, Date, Slot, Sources (multi_select).

KakaoTalk relay is downstream of Telegram — bare-URL formatting in the brief survives the clipboard hop on Android.

---

## Source catalog

70+ feeds defined in the private operational repo's `sources.yaml`. The 2026-05-04 revision strengthens "freshest signal" capture via release.atom firehoses and new fetcher modes.

### AI Trend Primary

| ID | Type | Weight | Parameters |
|---|---|---|---|
| `hf_papers` | HuggingFace Daily Papers | **1.7** | 2-day lookback, min_upvotes=15 |
| `hf_models_new` | HuggingFace Models (firehose) | **1.5** | New 2026-05-04. Iterates each foundation lab via author filter, sorts `createdAt desc`, top N per author, 7-day window. min_likes/downloads=0. |
| `github_trending_python` | GitHub Trending | 1.4 | language=python, since=daily, top 25 |
| `github_trending_overall` | GitHub Trending | 1.3 | all languages, since=daily, top 15 |
| `hf_models_trending` | HuggingFace Models | 0.9 | Demoted 2026-05-04 (1.3 → 0.9). Cumulative-popularity signal, weak on freshness. `hf_models_new` shares the load. |

**Foundation lab whitelist**: 31 orgs added 2026-05-04 — Western (CohereForAI, BlackForestLabs, NousResearch, apple, RekaAI, ai21labs, tiiuae, EleutherAI, bigscience, bigcode, Salesforce, ServiceNow, HuggingFaceH4/TB, Nexusflow), Chinese (moonshotai, Skywork, ZhipuAI, IEITYuan, StepFun-AI, 01-ai, THUDM, OpenBMB, internlm, baichuan-inc, Tencent, Alibaba-NLP, ByteDance), Korean (naver-hyperclovax, kakaocorp). Non-Western labs receive a 1.15× score boost.

### AI Lab Direct

| ID | Type | Weight | Feed |
|---|---|---|---|
| `anthropic_news` | RSS | **1.6** | anthropic.com/news/rss.xml |
| `openai_blog` | RSS | 1.5 | openai.com/blog/rss.xml |
| `meta_ai` | RSS | 1.4 | ai.meta.com/blog/rss/ — FAIR / Llama / SAM |
| `googleai_blog` | RSS | 1.3 | blog.google/technology/ai/rss/ |
| `deepmind_blog` | RSS | 1.3 | deepmind.google/blog/rss.xml |
| `huggingface_blog` | RSS | 1.3 | huggingface.co/blog/feed.xml |
| `mistral` | RSS | 1.3 | mistral.ai/news/feed.xml |

### Curators & AI Newsletters

| ID | Type | Weight | Feed |
|---|---|---|---|
| `simonwillison` | RSS | 1.4 | simonwillison.net/atom/everything/ |
| `import_ai` | RSS | 1.4 | importai.substack.com/feed — Jack Clark weekly digest |
| `latent_space` | RSS | 1.3 | latent.space/feed |
| `interconnects` | RSS | 1.3 | interconnects.ai/feed |
| `smol_ai` | RSS | 1.2 | buttondown.email/ainews/rss |

### Open Source Releases (Atom firehose)

GitHub `releases.atom` updates within seconds of a tag push — effectively real-time and the earliest first-party broadcast surface for software releases. The 2026-05-04 revision adds 14 LLM-infra / agent-framework / MCP SDK feeds in one batch.

| ID | Type | Weight | Feed |
|---|---|---|---|
| `claude_code_releases` | Atom | **1.5** | anthropics/claude-code |
| `mcp_releases` | Atom | 1.3 | modelcontextprotocol/specification |
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

### Korean AI Ecosystem

| ID | Type | Weight | Feed |
|---|---|---|---|
| `upstage_blog` | RSS | 1.2 | upstage.ai/blog/rss.xml — Solar model, document AI |
| `lg_ai_research` | RSS | 1.1 | lgresearch.ai/blog/rss.xml — EXAONE |
| `naver_d2` | Atom | 1.0 | d2.naver.com/d2.atom — CLOVA / HyperCLOVA |
| `kakao_tech` | RSS | 1.0 | tech.kakao.com/feed/ — Kakao Brain |

### Raw Database

| ID | Type | Weight | Parameters |
|---|---|---|---|
| `arxiv_ai` | arXiv API | 0.8 | categories=cs.AI/cs.LG/cs.CL/stat.ML, max_results=40, sort=submittedDate desc, rate_limit=3.0s |

### Aggregators

| ID | Type | Weight | Parameters |
|---|---|---|---|
| `hn_top` | Hacker News (Algolia, popularity) | 1.2 | tags=story, ai_min_points=120 / general_min_points=150, min_num_comments=20, lookback=30h |
| `hn_breaking` | Hacker News (Algolia, by date) | **1.4** | New 2026-05-04. `/api/v1/search_by_date`-based 4-hour firehose with a 75-point threshold for AI-keyword matches — catches early momentum before stories trend. |
| `techmeme` | RSS | 0.7 | techmeme.com/feed.xml (1.0 → 0.7, rumor + politics noise discount) |
| `producthunt` | RSS | 0.5 | producthunt.com/feed, max_items=15, min_votes=300 |

`hn_top` captures cumulative-popularity stories (already trending); `hn_breaking` captures early-momentum stories (becoming a story right now). The two channels are orthogonal on the time axis.

### X curation (limited)

A small set of personally-curated X follows plus a few official lab accounts is read on a 12-hour lookback per slot. The official X API is **not** used — instead, a small self-hosted relay returns raw text only. This is a slot-by-slot snapshot rather than a live firehose, but it covers cases where lab announcements hit X before the company blog. Curation is delegated to the user's own follow list — channel rotation needs no code change.

### Longform & Essays

| ID | Type | Weight | Feed |
|---|---|---|---|
| `oneusefulthing` | RSS | 1.3 | oneusefulthing.org/feed — Ethan Mollick, applied AI essays |
| `pragmaticengineer` | RSS | 1.2 | newsletter.pragmaticengineer.com/feed — Gergely Orosz, software engineering longform |
| `thebrowser` | Gmail Newsletter | 1.4 | label=Newsletter, from=caroline@thebrowser.com |
| `notboring` | Gmail Newsletter | 1.2 | label=Newsletter, from=packy@notboring.co |

### Meta-Source

| ID | Type | Weight | Feed |
|---|---|---|---|
| `lesswrong` | RSS | 1.1 | lesswrong.com/feed.xml?karmaThreshold=60, max=15 |
| `alignment_forum` | RSS | 1.0 | alignmentforum.org/feed.xml?karmaThreshold=20, max=10 |

### YouTube

| ID | Type | Weight | Channels |
|---|---|---|---|
| `youtube_ai` | YouTube RSS | 0.9 | Yannic Kilcher · Andrej Karpathy · Two Minute Papers · Latent Space · Dwarkesh Patel · Lex Fridman (4 items per channel, 48h lookback) |

**AI classification rule** — items from `{hf_papers, hf_models_trending, arxiv_ai, anthropic_news, openai_blog, googleai_blog, deepmind_blog, huggingface_blog, meta_ai, mistral, simonwillison, latent_space, interconnects, smol_ai, import_ai, alignment_forum, github_trending_python, github_trending_overall, oneusefulthing, claude_code_releases, mcp_releases, upstage_blog, lg_ai_research}` are AI by definition. Items from other sources are AI if title + body excerpt match `profile.md` AI Keywords; else general.

---

## Scoring formula

Per-item composite score, computed by Claude in Step 6 of `prompts/curate.md`. As of 2026-05-04, **6-axis with novelty added**:

```
total = 0.25·signal       (source_weight × normalized source score)
      + 0.25·affinity     (semantic match against profile.md)
      + 0.20·recency      (hour-resolution decay step function)
      + 0.20·novelty      (first-broadcast vs trending piggyback)
      + 0.05·velocity     (normalized HN/HF velocity)
      + 0.05·freshness    (penalty for 14-day brief overlap)
```

Recency step function:

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
   hf_papers · hn_breaking · x_lab)
Curator / aggregator                    65
Trending (artifact re-surfacing)     age-graded
  ≤7d                                   80
  ≤30d                                  50
  ≤90d                                  25
  >90d                                   0
arXiv (lagging raw DB)                  50
```

This axis quantitatively separates "GitHub trending #1, but actually a 2-year-old repo riding a one-day hype" from "nvidia just pushed a release" — roughly trending 1.0× / first-broadcast 1.6×.

**Structural cap** (Step 7 shortlist): trending sources (`github_trending_*`, `hf_models_trending`) are hard-capped at 2 items per AI section / 1 per General section. This is enforced in code before Step 8 (brief authoring), so "old trending dominates the brief" is blocked at the pipeline level.

Selection thresholds: AI items with score < 60 are dropped or count reduced; general items with score < 50 cause the General section to be omitted entirely. Source-diversity guard: max 2 items per `source_id` per section.

---

## Tech stack

| Layer | Choice | Why |
|---|---|---|
| Runtime | **Python 3.11+** with `from __future__ import annotations` everywhere | Type-hinted, dataclass-based items, available in Claude Code Routine cloud env |
| Orchestrator | **Claude Code Routines** on cloud | Cron-driven, fresh repo clone per run, native `git push` access |
| Parallelism | **`concurrent.futures.ThreadPoolExecutor`** (5 fetchers, 8 enrich workers) | I/O-bound — threads are sufficient, no asyncio complexity |
| RSS / Atom | **`feedparser` ≥ 6.0.10** | Tolerant of malformed feeds, normalizes Atom/RSS variants |
| HTML extraction | **`trafilatura` ≥ 1.12** + `BeautifulSoup4` + `lxml` | Highest-precision article body extraction in the open-source category |
| HTTP | **`requests` ≥ 2.31** with 20s timeout default | Per-call timeout, structured error handling |
| Time | **`python-dateutil` ≥ 2.8** | Robust ISO-8601 + RFC-822 parsing across feeds |
| Config | **`pyyaml` ≥ 6.0** for `sources.yaml` and `profile.md` | Human-editable, push-to-deploy |
| Delivery | **Telegram Bot API** (HTML mode, sendMessage) + **Notion API** (`notion-client` ≥ 2.2) | Telegram for push, Notion for archive — both free |
| Brief authoring | **Claude (in routine)** following `prompts/curate.md` Steps 6–8 | LLM does affinity scoring + prose composition; deterministic Python wraps it |
| Storage | Two-branch git: `main` for `state/seen.json`, `claude/brief-stream` for `briefs/*.md` + `state/raw/*.json` | Race-safe dedup state + Obsidian-followable archive |
| KakaoTalk relay | Android-side relay forwarding Telegram messages | No official Kakao open-chat push API exists |

No paid APIs. No OpenAI keys. No vector DB. No external scoring service. The pipeline is intentionally cheap and small.

---

## Failure modes & resilience

| Failure | Behavior |
|---|---|
| Source 5xx / timeout | Fetcher returns `[]` + warning log. Orchestrator records `failures[source_id]`. Other sources continue. |
| arXiv rate limit (3s/req) | Per-source `RateLimiter` enforces inter-request delay. |
| HuggingFace API schema change | `hf_papers` URL normalization to canonical `arxiv.org/abs/...` so cross-source dedup with `arxiv_ai` still works. |
| Telegram chunk send failure | Per-chunk try/except + 1 retry with backoff. Failed chunk does not block subsequent chunks. |
| Notion API 5xx | Logged, brief is still delivered to Telegram. Notion archive is best-effort. |
| Gmail credentials missing | `gmail_newsletter` fetcher returns `[]` + info log. |
| YouTube channel ID 404 | Per-channel try/except, other channels in `youtube_ai` continue. |
| Routine repo race | Routine pulls `--rebase` before push of `state/seen.json`; brief-stream uses ff-only merge from main. |
| Single bad URL in enrich | Per-item try/except, leaves `body_excerpt=""`, scoring continues. |
| No env vars (TELEGRAM_*) | `deliver.py` warns and exits 0 (does not crash the routine). |

---

## Output format

Brief markdown structure (slot file at `briefs/$DATE-$SLOT.md`):

```markdown
# Trendchaser: 4월 30일 10시 기준 최신 AI/Dev 소식

> 생성 2026-04-30 10:02 KST · 직전 슬롯 이후 13시간 스캔 · raw 201 → dedup 190 → 선정 8

## 🤖 AI

**Anthropic, 새 모델 SDK 공개**

Anthropic이 자체 에이전트 SDK를 정식 공개했다. ...
(출처: anthropic_news — https://www.anthropic.com/news/agent-sdk — 2시간 전)

(2–4 more items, blank line between each)

## 🌐 General

(1–2 items, same format)

## 📌 다음 슬롯에서 확인할 것

(50–60점대 대기 항목 한 단락, 또는 섹션 생략)

---
*Failed sources: 없음*
*Diagnostics: top_signal=anthropic_news(2.43), oldest_picked=8h ago*
```

The outbound message to Telegram has the `## 📌` preview and the diagnostics footer stripped (`strip_outbound_footer`). The brief file and the Notion archive retain everything for history.

---

## Telemetry & validation

Phase-09 dry-run validation (no live credentials):

| Test | Result |
|---|---|
| Scenario A — full fetch (morning slot, no env vars) | **201 items, 17 active sources, 0 failures** |
| Scenario B — fetch_all/dedupe/enrich/deliver chain | each step `exit 0` |
| Scenario C — `hf_papers + reddit_ml` disabled | other 16 sources continue, **166 items** |
| Scenario D — equal score, different velocity | fresh velocity preferred (1.300 vs 1.006) |
| Top-30 enrichment wall time | ≈ 3 s |
| `tests/test_delivery.py` | **14/14 pass** (escape, link with underscore, chunk paragraph split, hard split, notion blocks, env-missing exit 0) |
| arxiv body extraction parity (summary == body_excerpt) | 37/37 = **100%** |
| Telegram self-test (HTML escape, no double-escape, link with underscore URL) | pass |

Live operation: routine runs four times a day at 10:00 / 14:00 / 18:00 / 22:00 KST; first sample brief delivered as a single 3478-char Telegram chunk.

---

## Design principles

- **News-brief tone, not LinkedIn tone.** Headline + 3–5 plain sentences. No bullet salad, no "thought leadership," no empty adjectives like "groundbreaking."
- **Hour-resolution recency.** A story published 3 hours ago beats one from yesterday — even if yesterday's was technically "more relevant." Stale wins are not wins.
- **Cumulative deduplication.** A 14-day rolling window over every brief ever sent. Repeats die at source.
- **Source URL integrity.** Only URLs the fetcher actually collected. No synthesizing, no shortening, no replacing with search results. If verification fails, the item is dropped.
- **Failure isolation.** A 503 on HuggingFace does not take down the GitHub trending feed. Each source is best-effort; the pipeline ships what it has.
- **Push to deploy.** Edit `profile.md` or `sources.yaml`, push to `main`, the next slot picks up the change. No restart, no redeploy.

---

## Project status

Live operation. The routine runs four times a day at 10:00 / 14:00 / 18:00 / 22:00 KST and broadcasts to the KakaoTalk open chat linked above.

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
