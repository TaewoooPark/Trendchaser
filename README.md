# Trendchaser

**A prism for the AI signal stream — disperses noise, converges what matters.**

<p align="center">
  <img src="https://img.shields.io/github/license/TaewoooPark/Trendchaser?style=flat-square&labelColor=000000&color=333333" alt="License">
  <img src="https://img.shields.io/github/stars/TaewoooPark/Trendchaser?style=flat-square&logo=github&logoColor=white&labelColor=000000&color=333333" alt="GitHub stars">
  <img src="https://img.shields.io/github/last-commit/TaewoooPark/Trendchaser?style=flat-square&labelColor=000000&color=333333" alt="Last commit">
  <img src="https://img.shields.io/badge/status-live-000000?style=flat-square&labelColor=000000&color=333333" alt="Status">
  &nbsp;
  <img src="https://img.shields.io/badge/Python-000000?style=flat-square&logo=python&logoColor=white&labelColor=000000" alt="Python">
  <img src="https://img.shields.io/badge/Claude%20Code-000000?style=flat-square&logo=anthropic&logoColor=white&labelColor=000000" alt="Claude Code">
  <img src="https://img.shields.io/badge/Telegram-000000?style=flat-square&logo=telegram&logoColor=white&labelColor=000000" alt="Telegram">
  <img src="https://img.shields.io/badge/KakaoTalk-000000?style=flat-square&logo=kakaotalk&logoColor=white&labelColor=000000" alt="KakaoTalk">
  &nbsp;
  <img src="https://img.shields.io/badge/HuggingFace-000000?style=flat-square&logo=huggingface&logoColor=white&labelColor=000000" alt="HuggingFace">
  <img src="https://img.shields.io/badge/arXiv-000000?style=flat-square&logo=arxiv&logoColor=white&labelColor=000000" alt="arXiv">
  <img src="https://img.shields.io/badge/GitHub%20Trending-000000?style=flat-square&logo=github&logoColor=white&labelColor=000000" alt="GitHub Trending">
  <img src="https://img.shields.io/badge/Hacker%20News-000000?style=flat-square&logo=ycombinator&logoColor=white&labelColor=000000" alt="Hacker News">
</p>

> *"Reading all of it is impossible. Reading none of it means missing the actual shifts."*

[한국어 README](./README.ko.md) &nbsp;·&nbsp; [**taewoopark.com** — author site](https://taewoopark.com)

<p align="center">
  <img src="./assets/logo.png" alt="Trendchaser logo — concentric points dispersing and converging like light through a prism." width="320">
</p>

---

> **This repository documents the technical background of Trendchaser.**
> The actual live briefs are delivered to a public KakaoTalk open chat (anonymous join allowed):
> **→ https://open.kakao.com/o/pfQMgHsi**
>
> Source code is kept private. This repository is documentation only.

---

## Why Trendchaser?

The daily AI/dev firehose is too wide to read and too important to ignore. Most newsletters either hand you a bullet list and call it curation, or wrap empty buzzwords around stale links.

| Traditional AI newsletter | Trendchaser |
|---|---|
| Daily digest of links | Three short briefs a day, hour-resolution recency |
| Bullet list, no context | Headline + 3–5 plain sentences, source link |
| Same story for a week | 14-day cumulative dedup — repeats killed at source |
| One feed dies, the page is empty | Failure isolation — the others still ship |
| Synthesized URLs from search | Only URLs the fetcher actually collected |

The core idea is **disperse → score → converge**: pull the full spectrum of sources, drop the wavelengths that don't matter, and reassemble what's left into something a person can read in two minutes.

---

## Why a prism?

A prism takes a single beam of white light and **disperses** it into the spectrum that was already inside — then a second prism **converges** the parts you actually need back into a usable beam.

Trendchaser is that second prism for your daily AI/dev feed.

The beam arriving each morning is already too wide: HuggingFace papers, GitHub trending, Anthropic and OpenAI posts, arxiv, Hacker News, curators like Simon Willison, lab newsletters, the better Substacks. Trendchaser stands at the focal point: it disperses the full spectrum, drops the wavelengths that don't matter, and converges what's left into three short briefs a day.

```
   sources ──┐
   sources ──┤        ╱╲          ╲   ╱
   sources ──┼──────▶ ╱  ╲ ──────▶ ╲ ╱ ──▶  brief
   sources ──┤       ╱    ╲        ╳
   sources ──┘      ╱──────╲      ╱ ╲
                  disperse + score   converge
```

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

## How it arrives

Three slots a day, Korea time.

| Slot | When (KST) | What's in it |
|---|---|---|
| **morning** | 10:00 | 5 AI + 3 general — the overnight wave from US/EU |
| **afternoon** | 15:00 | 3 AI + 2 general — what broke during your day |
| **evening** | 22:00 | 3 AI + 2 general — Asia day-end + early US morning |

A scheduled job in the cloud runs the pipeline at each slot — fetch sources, drop duplicates, score against a curation profile, write the brief, send it. The brief lands first in Telegram, then is relayed into the KakaoTalk open chat above.

If a story already appeared in any brief in the last 14 days, it won't appear again. If a source goes down, the others still ship.

---

## What's behind it

A small Python pipeline that runs in [Claude Code Routines](https://claude.com/claude-code) on the cloud:

| Stage | What it does |
|---|---|
| **1. Fetch** | 16 active sources in parallel — HuggingFace, arxiv, GitHub Trending, lab blogs, curator feeds, YouTube, newsletters |
| **2. Deduplicate** | Across sources and across the last 14 days of briefs, by canonical URL + normalized title |
| **3. Enrich** | Pull the body of the top ~30 candidates so scoring reads the actual content, not just the headline |
| **4. Score** | signal × source weight, semantic affinity to a curation profile, hour-resolution recency decay, velocity, freshness penalty |
| **5. Write** | Brief composed by the routine itself in news-brief tone — headline, then paragraph |
| **6. Deliver** | Telegram (HTML, chunked under 4096 chars) → KakaoTalk open chat relay → optional Notion archive |

The full pipeline is implemented as a 12-step curation routine that runs three times a day without human intervention.

---

## Design principles

- **News-brief tone, not LinkedIn tone.** Headline + 3–5 plain sentences. No bullet salad, no "thought leadership," no empty adjectives like "groundbreaking."
- **Hour-resolution recency.** A story published 3 hours ago beats one from yesterday — even if yesterday's was technically "more relevant." Stale wins are not wins.
- **Cumulative dedup.** A 14-day rolling window of every brief ever sent. Repeats are killed at the source.
- **Source URL integrity.** Only URLs the fetcher actually collected. No synthesizing, no shortening, no replacing with search results. If verification fails, the story is dropped.
- **Failure isolation.** A 503 on HuggingFace doesn't take down the GitHub trending feed. Each source is best-effort, the pipeline ships what it has.

---

## Project status

Live operation. The routine runs three times a day at 10:00 / 15:00 / 22:00 KST and broadcasts to the KakaoTalk open chat linked above.

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
</p>

Other things I'm building:
[NODEPROMPT](https://github.com/TaewoooPark/NODEPROMPT) &nbsp;·&nbsp; [PAIDEIA](https://github.com/TaewoooPark/PAIDEIA) &nbsp;·&nbsp; [PAIDEIA-codex](https://github.com/TaewoooPark/PAIDEIA-codex) &nbsp;·&nbsp; [taewoopark.com](https://github.com/TaewoooPark/taewoopark.com)

---

<p align="center">
  <sub>Trendchaser — disperse, score, converge.</sub>
</p>
