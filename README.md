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

🧪 Side Project (별도 레포)
└─ NoteBook_MOD       # ↑ Project 3에서 모듈화 한계를 느껴 직접 만든 JupyterLab Extension
                      # → https://github.com/popixoxipop-collab/NoteBook_MOD
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

## 🧪 Side Project — NoteBook_MOD

> **Project 3 진행 중 부딪힌 한계에서 출발한 사이드 프로젝트.**
> 🔗 https://github.com/popixoxipop-collab/NoteBook_MOD

### 🩹 동기 — Project 3에서 느낀 모듈화의 벽

Project 3의 `Step1_상품리뷰분석_Agent_1_완성.ipynb`는 **62개 셀**에
ReviewState · analyzer_node · critic_node · supervisor_node · DB CRUD · Streamlit UI · 실행 테스트가
**전부 한 파일**에 뒤섞여 있었다. 더구나 `app.py`(Streamlit)에는 Agent 코드가 통째로 복붙되어 중복까지 발생.

| 위반된 설계 원칙 | 노트북에서의 증상 |
|---|---|
| 단일 책임(SRP) | 분석·검증·DB·UI가 같은 노트북 |
| 관심사 분리(SoC) | Agent 로직 + Streamlit UI 같은 셀 |
| 재사용성 | `app.py`에 Agent 코드 복붙 |
| 테스트 가능성 | 함수 개별 import 불가 |

기존 도구(`nbconvert`, `jupytext`, `Soorgeon`, `nbrefactor`)는 모두 한계가 있었다 — **변환은 되지만 "함수/레이어별로 자동 분리하고 import까지 연결"해주는 도구는 없었다.** 그래서 직접 만들기로 함.

### 💡 솔루션 — 노트북 → 자동 모듈화 JupyterLab Extension

```
[Before — 1 ipynb, 62 cells]              [After — 패키지 구조]

Step1_..._완성.ipynb                       myproject/
├─ ReviewState                             ├─ state.py             ← ReviewState
├─ analyzer_node                           ├─ agents/
├─ critic_node                             │   ├─ analyzer_node.py
├─ supervisor_node                         │   ├─ critic_node.py
├─ DB 연결/생성/저장                       │   └─ supervisor_node.py
├─ Streamlit UI                            ├─ graph/
└─ 실행 테스트                             │   ├─ build_graph.py
   (전부 한 파일, 중복 포함)              │   └─ route_next.py
                                           ├─ db/
                                           │   ├─ init_db.py
                                           │   └─ save_result.py
                                           └─ utils/
```

> 실제로 본 레포의 `extension/` 폴더에 위 구조 그대로 들어있다 — Project 3 노트북을 모듈화한 결과물이 그대로 dogfood 샘플.

### 🧠 핵심 알고리즘 — 3단계 분류 파이프라인

```
함수/클래스 추출 (CodeMirror 정규식)
        │
        ▼
┌─────────────────────────────────────────┐
│ Step 1. StateDB hit                     │
│   .nbmod_state.db 확정 매핑 즉시 반환    │  ← LLM 호출 0
│   (human-confirmed 경로 우선)            │
└──────────────┬──────────────────────────┘
               │ miss
               ▼
┌─────────────────────────────────────────┐
│ Step 2. OpenAI gpt-4o-mini 분류         │
│   확정 사례 + 수정 이력 few-shot 주입    │
│   JSON 응답: {path, confidence, reason} │
└──────────────┬──────────────────────────┘
               │ 실패/부재
               ▼
┌─────────────────────────────────────────┐
│ Step 3. 규칙 기반 Fallback              │
│   sentence-transformers / TF-IDF        │
│   + greedy cosine 클러스터링            │
│   + 함수명 정규식 (*_node → agents/...)  │
└─────────────────────────────────────────┘
```

### 🎨 핵심 UX — 인라인 모듈 뱃지

함수 본문이 사라지는 대신 **CodeMirror StateField + Widget으로 뱃지로 치환**된다.

```
[모듈화 전]                          [모듈화 후]

def analyzer_node(state):     →     def 📄 analyzer_node
    review = state["review"]              ↑ hover/click 가능
    system_prompt = """..."""             (본문 30줄 숨김)
    ...
```

| 동작 | 결과 |
|---|---|
| **hover** | 원본 함수 코드 툴팁 (syntax highlight) |
| **click** | 연결된 `.py` 파일을 JupyterLab 우측 패널에서 오픈 |
| **double-click** | 뱃지 풀려서 셀에 코드 인라인 전개 |
| **confidence < 0.8** | `nbmod-uncertain` 클래스로 시각 강조 |
| **human-confirmed** | `nbmod-confirmed` 클래스로 강조 |

### 🔁 Human-in-the-Loop — 사용자가 LLM 학습 데이터를 만든다

뱃지 클릭 시 드롭다운에서 카테고리를 수정할 수 있고, **수정 즉시 StateDB에 영속화** → 다음 분류부터 **few-shot 예시로 자동 주입**.

```sql
-- .nbmod_state.db 스키마
CREATE TABLE confirmed_mappings (
  func_name TEXT PRIMARY KEY,
  file_path TEXT NOT NULL,
  source    TEXT NOT NULL,    -- 'llm' | 'human'
  confidence REAL DEFAULT 1.0
);
CREATE TABLE corrections (
  id INTEGER PRIMARY KEY,
  func_name TEXT,
  from_path TEXT,
  to_path   TEXT             -- → 다음 LLM 프롬프트 few-shot
);
```

### 🛠️ 기술 스택

| 레이어 | 사용 기술 |
|---|---|
| **Extension Frontend** | TypeScript · JupyterLab 4.x · CodeMirror 6 (StateField, EditorView, ViewPlugin) |
| **Extension Backend** | Python · `jupyter-server` APIHandler · Tornado |
| **LLM** | OpenAI `gpt-4o-mini` (JSON 응답 강제, temperature 0) |
| **Embedding Fallback** | `sentence-transformers` (`all-MiniLM-L6-v2`) · TF-IDF (`char_wb` 3-5 n-gram) |
| **State** | SQLite (`.nbmod_state.db`, cwd-scoped) |
| **Build** | `hatchling` · `hatch-jupyter-builder` · yarn (jlpm) |
| **MVP 웹 버전 설계** | Next.js + Tailwind, FastAPI, React Flow, Vercel + Railway |

### 📂 레포 구조

```
NoteBook_MOD/
├─ MVP_DESIGN.md                 # 시장 분석·MVP 범위·아키텍처 11개 섹션
└─ extension/
   ├─ src/                       # TypeScript — CodeMirror StateField, 뱃지, 드롭다운
   │   ├─ index.ts               # 플러그인 진입점 (state-aware + correction UI)
   │   ├─ algorithm.ts           # 규칙 기반 경로 추론 (클라이언트 사이드)
   │   ├─ badge.ts               # CodeMirror Widget 뱃지
   │   ├─ viewPlugin.ts          # StateField + StateEffect
   │   ├─ stateStore.ts          # 백엔드 state API 클라이언트
   │   └─ correctionWidget.ts    # 수정 드롭다운 UI
   ├─ notebook_mod/              # Python — jupyter-server 핸들러
   │   ├─ handlers.py            # /analyze, /state, /categories
   │   ├─ llm_classifier.py      # OpenAI 분류기 (few-shot 빌더)
   │   └─ state_db.py            # SQLite 영속 상태
   └─ extension/                 # ★ Project 3 노트북을 모듈화한 dogfood 샘플
      ├─ state.py                # ReviewState
      ├─ agents/                 # analyzer/critic/supervisor 노드
      ├─ graph/                  # build_graph + route_next
      └─ db/                     # init_db + save_result
```

### 🎯 Project 3과의 연결

| Project 3 (노트북 안) | NoteBook_MOD (분리 결과) |
|---|---|
| `class ReviewState(TypedDict):` | `extension/state.py` |
| `def analyzer_node(state):` | `extension/agents/analyzer_node.py` |
| `def critic_node(state):` | `extension/agents/critic_node.py` |
| `def supervisor_node(state):` | `extension/agents/supervisor_node.py` |
| `builder = StateGraph(...)` | `extension/graph/build_graph.py` |
| `def route_next(state):` | `extension/graph/route_next.py` |
| SQLite 저장 코드 | `extension/db/save_result.py`, `init_db.py` |

→ **Project 3의 모듈화 한계가 곧 NoteBook_MOD의 검증 케이스가 된 구조.**

### 📈 MVP 성공 지표

| 지표 | 목표 |
|---|---|
| 셀 분류 정확도 | ≥ 85% (vs 사람) |
| import 자동 연결 성공률 | ≥ 80% |
| 62-cell 노트북 처리 시간 | ≤ 20초 |
| `python -c "import myproject"` 통과 | ≥ 70% |

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
