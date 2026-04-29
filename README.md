<div align="center">

<img src="./assets/logo.png" alt="Trendchaser" width="220" />

# Trendchaser

*A prism for the AI signal stream — disperses noise, converges what matters.*

[![Status](https://img.shields.io/badge/status-live-22c55e?style=flat-square)](#how-it-arrives)
[![Cadence](https://img.shields.io/badge/cadence-3%20briefs%2Fday-3b82f6?style=flat-square)](#how-it-arrives)
[![Delivery](https://img.shields.io/badge/delivery-Telegram%20%2B%20KakaoTalk-26a5e4?style=flat-square&logo=telegram&logoColor=white)](#how-it-arrives)
[![Built with](https://img.shields.io/badge/built%20with-Claude%20Code-d97757?style=flat-square)](https://claude.com/claude-code)
[![License](https://img.shields.io/badge/docs-MIT-fafafa?style=flat-square)](./LICENSE)
[![Last Commit](https://img.shields.io/github/last-commit/TaewoooPark/Trendchaser?style=flat-square&color=8b5cf6)](https://github.com/TaewoooPark/Trendchaser/commits)

[**한국어 README**](./README.ko.md)

</div>

---

> 🟢 **This repository documents the technical background of Trendchaser.**
> The actual live briefs are delivered to a public KakaoTalk open chat (anonymous join allowed):
> **→ https://open.kakao.com/o/pfQMgHsi**
>
> Source code is kept private. This repo is documentation only.

---

## What you receive

Three short news briefs a day, sent straight to your phone. Each one is a tight read — bold headline, three to five sentences of plain context, a source link. No bullet salad, no buzzword soup, no "thought leadership."

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

A prism takes a single beam of white light and **disperses** it into the spectrum that was already inside — then a second prism **converges** the parts you actually need back into a usable beam.

Trendchaser is that second prism for your daily AI/dev feed.

The beam arriving each morning is already too wide: HuggingFace papers, GitHub trending, Anthropic and OpenAI posts, arxiv, Hacker News, curators like Simon Willison, lab newsletters, the better Substacks. Reading all of it is impossible. Reading none of it means missing the actual shifts.

Trendchaser stands at the focal point: it disperses the full spectrum, drops the wavelengths that don't matter, and converges what's left into three short briefs a day.

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

Three slots a day, Korea time:

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

1. **Fetch** — 16 active sources in parallel (HuggingFace, arxiv, GitHub Trending, lab blogs, curator feeds, YouTube, newsletters)
2. **Deduplicate** — across sources and across the last 14 days of briefs, by canonical URL + normalized title
3. **Enrich** — pull the body of the top ~30 candidates so scoring reads the actual content, not just the headline
4. **Score** — signal × source weight, semantic affinity to a curation profile, recency (hour-resolution decay), velocity, freshness penalty for recently-shipped stories
5. **Write** — the brief is composed by the routine itself in news-brief tone, headline-then-paragraph
6. **Deliver** — Telegram (HTML, chunked under 4096 chars) → KakaoTalk open chat relay → optional Notion archive

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

Built by **Taewoo Park** — physics + math @ KAIST, research intern at the KAIST Ultrafast Spin Dynamics Lab, working on aligning physics, code, and culture toward a civilization-scale solution.

[![Website](https://img.shields.io/badge/Website-taewoopark.com-0a0a0a?style=flat-square&logo=safari&logoColor=white)](https://taewoopark.com)
[![GitHub](https://img.shields.io/badge/GitHub-TaewoooPark-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/TaewoooPark)
[![X](https://img.shields.io/badge/X-@theoverstrcture-000000?style=flat-square&logo=x&logoColor=white)](https://x.com/theoverstrcture)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Taewoo%20Park-0a66c2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/taewoo-park-427a05352)
[![Instagram](https://img.shields.io/badge/Instagram-@t.wo0__x-e4405f?style=flat-square&logo=instagram&logoColor=white)](https://www.instagram.com/t.wo0_x/)
[![Archive](https://img.shields.io/badge/Archive-@hustlyarchiv.kr-c026d3?style=flat-square&logo=instagram&logoColor=white)](https://www.instagram.com/hustlyarchiv.kr/)
[![Email](https://img.shields.io/badge/Email-ptw151125@kaist.ac.kr-ea4335?style=flat-square&logo=gmail&logoColor=white)](mailto:ptw151125@kaist.ac.kr)

Other things I'm building:
[NODEPROMPT](https://github.com/TaewoooPark/NODEPROMPT) · [PAIDEIA](https://github.com/TaewoooPark/PAIDEIA) · [PAIDEIA-codex](https://github.com/TaewoooPark/PAIDEIA-codex) · [taewoopark.com](https://github.com/TaewoooPark/taewoopark.com)

---

<div align="center">
<sub>Trendchaser — disperse, score, converge.</sub>
</div>
