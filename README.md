# Aivle School — AI Track Portfolio

> KT Aivle School AI 트랙 미니프로젝트 아카이브
> LangChain · LangGraph · LangSmith · Streamlit · Gradio 기반 LLM Agent 시스템 구축

---

## 👤 About

| | |
|---|---|
| **Name** | 노경천 |
| **Track** | KT Aivle School — AI |
| **Focus** | LLM Agent Systems, Multimodal Pipelines, LangGraph Orchestration |
| **Stack** | Python · LangChain · LangGraph · LangSmith · OpenAI · Streamlit · Gradio |

---

## 📂 Repository Structure

```
Aivle/
├── 미니프로젝트 1차/   # 팀 협업 · AI 기획 발표
├── 미니프로젝트 2차/   # AI 강사 Agent v2.0 (PPT → 강의 영상 자동화)
└── 미니프로젝트 3차/   # 상품리뷰분석 Agent v2.0 (Supervisor + Dashboard)
```

---

## 🧩 Project 1 — AI 기획 & 팀 협업

**`미니프로젝트 1차/`**

| Deliverable | File |
|---|---|
| 팀 회의록 | `18조_회의록.docx` |
| AI 발표 자료 | `AI_18조.pptx` |
| 개인 포트폴리오 (구조화) | `포트폴리오_구조화&경험_구체화_노경천.pptx` |

**Role.** 18조 — 기획·문서화 담당. AI 활용 시나리오 도출, 발표 자료 구성, 팀 회의 진행/기록.

---

## 🎓 Project 2 — AI 강사 Agent v2.0

**`미니프로젝트 2차/AI_06반_17조.ipynb`**

> 강의 PPT를 입력하면, 슬라이드별 강의 스크립트를 생성하고 TTS · 영상 합성 · Gradio 웹앱까지 한 번에 만드는 End-to-End LangGraph Agent.

### 🔧 Architecture

```
[PPT 입력]
   │
   ▼
[정보 분해] ── 슬라이드 텍스트·이미지·제목 추출
   │
   ▼
[Tavily 외부 검색] ── 부연 설명용 컨텍스트 수집
   │
   ▼
[페이지별 내용 생성] ── GPT-4 기반 강의 내용 작성
   │
   ▼
[전체 흐름 기반 스크립트] ── 강의 흐름 구상 → 페이지 스크립트
   │
   ▼
[Critic 노드] ── 내용 검토 / 흐름 적절성 검증
   │
   ▼
[TTS 음성 변환] ── 톤·스타일 조절
   │
   ▼
[영상 합성 (ffmpeg)] ── 슬라이드 스냅샷 + 오디오
   │
   ▼
[Gradio 웹 인터페이스]
```

### 🛠️ Tech

- **Orchestration** — LangGraph (StateGraph, 노드 분기·반복)
- **LLM** — `langchain-openai` (GPT-4 family)
- **Search** — `langchain-tavily` 외부 컨텍스트 보강
- **Media** — `python-pptx` · `Pillow` · `ffmpeg` · `poppler-utils` · `libreoffice`
- **UI** — Gradio
- **i18n** — Noto CJK / Devanagari / Arabic 폰트 (다국어 자막 지원)

### 🎯 Mission Coverage

- ✅ **미션③ 모듈 고도화 1** — 정보 분해, 페이지별 내용 생성 + 외부 검색, 강의 스크립트, 내용 검토
- ✅ **미션④ 모듈 고도화 2** — TTS 음성 변환, 페이지·전체 영상 제작, Gradio 연동, LangGraph 전체 그래프 구축

---

## 🛍️ Project 3 — 상품리뷰분석 Agent v2.0

**`미니프로젝트 3차/AI_15조.ipynb`**

> Supervisor 패턴 기반 멀티 에이전트로 상품 리뷰의 측면(aspect)별 감성을 분석하고, LangSmith로 trace를 관측하며 Streamlit 대시보드로 시각화한다.

### 🔧 Architecture

```
                     ┌────────────────────────────┐
                     │      Supervisor Agent      │
                     │  (반복 제어 · 라우팅 결정) │
                     └────┬───────────────────┬───┘
                          │                   │
                          ▼                   ▼
              ┌───────────────────┐  ┌───────────────────┐
              │  Analyzer Agent   │  │   Critic Agent    │
              │ aspect-level 감성 │  │ 검증 / 재요청      │
              └─────────┬─────────┘  └─────────┬─────────┘
                        │                      │
                        └──────────┬───────────┘
                                   ▼
                        ┌──────────────────────┐
                        │  SQLite DB Batch Loop │
                        │  리뷰 → 예측 저장     │
                        └──────────┬───────────┘
                                   ▼
                        ┌──────────────────────┐
                        │  Streamlit Dashboard  │
                        │  집계 · 시각화 · 조회 │
                        └──────────────────────┘

                  Observed by: LangSmith Tracing
```

### 🛠️ Tech

- **Multi-Agent** — Analyzer · Critic · Supervisor 노드, ReviewState (TypedDict) 공유
- **LLM** — `gpt-4.1-mini` (temperature 0.2)
- **Observability** — LangSmith (`LANGSMITH_TRACING=true`, project=`proj2_agent`)
- **Storage** — SQLite (리뷰 + 분석 결과 batch 저장)
- **Dashboard** — Streamlit + matplotlib/seaborn (감성 분포·집계·드릴다운)
- **Data** — `data_gender.csv`, Step2 설계 산출물 (Agent 역할 정의서, Workflow 명세서)

### 🎯 Mission Coverage

- ✅ **미션③ LangSmith 모니터링** — Trace 설정, 평가 데이터셋, 실패 케이스 추적
- ✅ **미션④ Agent 고도화** — Supervisor 중심 반복 제어, DB 리뷰 batch 처리
- ✅ **미션⑤ 대시보드** — Streamlit 기반 리뷰 결과 집계 + 개별 조회

### 📐 Design Artifacts

- `Step2_Agent 역할 정의서_양식.xlsx` — 에이전트별 입력/출력/책임 정의
- `Step2_Workflow 명세서 외 기타 설계문서들_양식.xlsx` — 노드·엣지·상태 명세

---

## 🧠 Skills Demonstrated

| Domain | Skills |
|---|---|
| **LLM Engineering** | Prompt design, structured output (JSON/TypedDict), multi-turn agent control |
| **Orchestration** | LangGraph StateGraph, Supervisor pattern, conditional edges, 반복·종료 분기 |
| **Observability** | LangSmith tracing, run inspection, failure case analysis |
| **Multimodal** | PPT 파싱, 이미지/오디오/영상 합성, TTS, 다국어 자막 |
| **Data** | SQLite batch I/O, pandas/seaborn 시각화 |
| **Product** | Gradio · Streamlit UI 프로토타이핑, end-user 인터페이스 |
| **Collaboration** | 팀 단위 기획·회의록·발표 자료 작성 |

---

## 🚀 How to Run

각 노트북은 **Google Colab** 기준으로 작성됨.

```bash
# 1) Colab에서 노트북 열기
# 2) Google Drive 마운트 (proj1_agent / proj2_agent 폴더)
# 3) api_key.txt 업로드
#    OPENAI_API_KEY=sk-...
#    TAVILY_API_KEY=tvly-...
#    LANGSMITH_API_KEY=ls-...  (Project 3 only)
# 4) 모든 셀 순차 실행
```

**Required:** Python 3.10+, OpenAI API key, Tavily API key (Project 2), LangSmith API key (Project 3).

---

## 📜 License

학습 목적의 미니프로젝트 산출물입니다. 외부 사용/배포 시 작성자에게 문의 바랍니다.
