<div align="center">

<img src="./assets/logo.png" alt="Trendchaser" width="220" />

# Trendchaser

*AI 신호의 흐름을 위한 프리즘 — 잡음을 흩뜨리고, 의미 있는 것만 다시 모은다.*

[![Status](https://img.shields.io/badge/status-live-22c55e?style=flat-square)](#어떻게-도착하는가)
[![Cadence](https://img.shields.io/badge/cadence-하루%203회-3b82f6?style=flat-square)](#어떻게-도착하는가)
[![Delivery](https://img.shields.io/badge/delivery-Telegram%20%2B%20KakaoTalk-26a5e4?style=flat-square&logo=telegram&logoColor=white)](#어떻게-도착하는가)
[![Built with](https://img.shields.io/badge/built%20with-Claude%20Code-d97757?style=flat-square)](https://claude.com/claude-code)
[![License](https://img.shields.io/badge/docs-MIT-fafafa?style=flat-square)](./LICENSE)
[![Last Commit](https://img.shields.io/github/last-commit/TaewoooPark/Trendchaser?style=flat-square&color=8b5cf6)](https://github.com/TaewoooPark/Trendchaser/commits)

[**English README**](./README.md)

</div>

---

> 🟢 **본 레포는 Trendchaser의 기술적 배경을 설명하기 위한 문서를 담고 있습니다.**
> 실제 서빙되는 실시간 정보들은 카카오톡 오픈채팅방에서 확인할 수 있습니다 (익명 가입 가능):
> **→ https://open.kakao.com/o/pfQMgHsi**
>
> 소스 코드는 비공개입니다. 본 레포는 문서 전용입니다.

---

## 받는 사람의 입장에서

하루 세 번, 짧은 뉴스 브리프가 폰으로 도착한다. 한 항목은 굵은 헤드라인 한 줄과 3–5문장의 줄글, 그리고 출처 링크. bullet 나열도, 알맹이 없는 트렌드 워드도, "thought leadership" 같은 공허한 표현도 없다.

```
🤖 AI

Anthropic, 새 모델 SDK 공개

Anthropic이 자체 에이전트 SDK를 정식 공개했다. 기존 API
위에서 도구 호출과 메모리를 한 단계 추상화한 것으로, ...
(출처: anthropic_news — https://anthropic.com/news/... — 2시간 전)

🌐 General

...
```

신문 읽듯 읽으면 된다. 직접 돌리는 건 아무것도 없다.

---

## 왜 prism인가

프리즘은 백색광 안에 이미 들어 있던 스펙트럼을 **분산**시켜 보여 주고, 두 번째 프리즘이 그중 필요한 파장만 다시 **수렴**시켜 쓸 수 있는 빛으로 만든다.

Trendchaser는 당신의 일일 AI/Dev 피드에 대해 그 두 번째 프리즘 역할을 한다.

매일 아침 도착하는 빔은 이미 너무 넓다. HuggingFace 논문, GitHub trending, Anthropic·OpenAI 포스트, arxiv, Hacker News, Simon Willison 같은 큐레이터, lab newsletter, 괜찮은 Substack까지. 다 읽는 건 불가능하고, 다 안 읽으면 진짜로 일어나고 있는 변화를 놓친다.

Trendchaser는 그 초점에 선다. 전체 스펙트럼을 펼치고, 의미 없는 파장은 떨어뜨리고, 남은 것을 하루 세 개의 짧은 브리프로 다시 모은다.

```
   sources ──┐
   sources ──┤        ╱╲          ╲   ╱
   sources ──┼──────▶ ╱  ╲ ──────▶ ╲ ╱ ──▶  brief
   sources ──┤       ╱    ╲        ╳
   sources ──┘      ╱──────╲      ╱ ╲
                  분산 + 점수화      수렴
```

---

## 어떻게 도착하는가

하루 세 슬롯 (KST):

| 슬롯 | 시각 | 내용 |
|---|---|---|
| **morning** | 10:00 | AI 5 + general 3 — 미국·유럽이 밤사이 만든 흐름 |
| **afternoon** | 15:00 | AI 3 + general 2 — 낮 동안 새로 터진 것 |
| **evening** | 22:00 | AI 3 + general 2 — 아시아 마감 + 미국 오전 시작 |

각 슬롯마다 클라우드의 스케줄러가 파이프라인을 돌린다 — 수집, 중복 제거, 큐레이션 프로필 기준 점수 매기기, 브리프 작성, 전송. 브리프는 먼저 Telegram에 도착하고, 그 다음 위 KakaoTalk 오픈채팅으로 relay된다.

최근 14일 안에 어느 브리프에든 한 번 등장한 기사는 다시 등장하지 않는다. 한 소스가 죽어도 나머지는 그대로 송출된다.

---

## 내부 구조

[Claude Code Routines](https://claude.com/claude-code)가 클라우드에서 돌리는 작은 Python 파이프라인:

1. **Fetch** — 16개의 active source를 병렬 수집 (HuggingFace, arxiv, GitHub Trending, lab 블로그, 큐레이터 피드, YouTube, newsletter)
2. **Deduplicate** — 소스 간, 그리고 최근 14일치 브리프에 대해, canonical URL + normalized title 기준
3. **Enrich** — 상위 ~30개 후보의 본문을 추출해 점수화가 헤드라인이 아니라 실제 내용을 읽도록
4. **Score** — signal × source weight, 큐레이션 프로필과의 의미 유사도 (affinity), recency (시간 단위 점감), velocity, 최근 등장 페널티 (freshness)
5. **Write** — 브리프 본문은 routine이 직접 뉴스 톤으로 작성. 헤드라인 → 줄글 단락 구조
6. **Deliver** — Telegram (HTML, 4096자 미만 chunk) → KakaoTalk 오픈채팅 relay → 선택적 Notion 아카이브

전체 파이프라인은 사람 개입 없이 하루 세 번 자동 실행되는 12-step 큐레이션 루틴으로 구현되어 있다.

---

## 설계 원칙

- **뉴스 브리프 톤, LinkedIn 톤이 아님.** 헤드라인 + 3–5문장 줄글. bullet 나열·"thought leadership"·"획기적인" 같은 공허한 형용사 없음.
- **시간 단위 신선도.** 3시간 전 기사가 어제 기사를 이긴다 — 어제 게 기술적으로 "더 적합"하더라도. 한물간 승리는 승리가 아니다.
- **누적 dedup.** 송출된 모든 브리프에 대한 14일 rolling window. 중복은 소스 단계에서 제거.
- **출처 URL 무결성.** 수집기가 실제로 모은 URL만. 합성·단축·검색 결과 대체 금지. 검증 실패 시 항목 자체 제외.
- **장애 격리.** HuggingFace 503이 GitHub trending 피드를 끌고 내려가지 않는다. 각 소스는 best-effort, 파이프라인은 가진 것을 송출한다.

---

## 진행 상태

라이브 운영 중. 매일 KST 10:00 / 15:00 / 22:00에 routine이 실행되어 위 KakaoTalk 오픈채팅으로 송출됩니다.

---

## Author

**Taewoo Park** — KAIST 물리학·수학과학 복수전공, KAIST Ultrafast Spin Dynamics Lab 연구 인턴. 물리·코드·문화를 정렬해 문명 단위의 솔루션으로 가는 길을 만든다.

[![Website](https://img.shields.io/badge/Website-taewoopark.com-0a0a0a?style=flat-square&logo=safari&logoColor=white)](https://taewoopark.com)
[![GitHub](https://img.shields.io/badge/GitHub-TaewoooPark-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/TaewoooPark)
[![X](https://img.shields.io/badge/X-@theoverstrcture-000000?style=flat-square&logo=x&logoColor=white)](https://x.com/theoverstrcture)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Taewoo%20Park-0a66c2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/taewoo-park-427a05352)
[![Instagram](https://img.shields.io/badge/Instagram-@t.wo0__x-e4405f?style=flat-square&logo=instagram&logoColor=white)](https://www.instagram.com/t.wo0_x/)
[![Archive](https://img.shields.io/badge/Archive-@hustlyarchiv.kr-c026d3?style=flat-square&logo=instagram&logoColor=white)](https://www.instagram.com/hustlyarchiv.kr/)
[![Email](https://img.shields.io/badge/Email-ptw151125@kaist.ac.kr-ea4335?style=flat-square&logo=gmail&logoColor=white)](mailto:ptw151125@kaist.ac.kr)

함께 만드는 다른 것들:
[NODEPROMPT](https://github.com/TaewoooPark/NODEPROMPT) · [PAIDEIA](https://github.com/TaewoooPark/PAIDEIA) · [PAIDEIA-codex](https://github.com/TaewoooPark/PAIDEIA-codex) · [taewoopark.com](https://github.com/TaewoooPark/taewoopark.com)

---

<div align="center">
<sub>Trendchaser — 분산하고, 점수 매기고, 수렴한다.</sub>
</div>
