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

---

## AWS 미니프로젝트 발표 스크립트

> KT 에이블스쿨 6반 17조 — AWS 인프라 미니프로젝트 발표 (2026)

---

### 1. 오프닝

안녕하세요. 6반 17조 AWS 미니프로젝트 발표를 시작하겠습니다.

---

### 2. 목차

1. EC2, ECS, EKS 각 운영환경의 시스템 아키텍처와 각 환경의 장단점
2. 일자별 결과물과 Git을 기준으로 통일 관리·자동화하는 운영 철학인 GitOps를 어떻게 구성했는지
3. 프로젝트 중 생긴 트러블슈팅

---

### 3. Executive Summary

- 저희 조의 경우 다양한 인프라 환경을 경험하며 프로젝트와 적합한 방식을 찾고자 했습니다.
- EC2, ECS, EKS를 경험해보며 최종적으로 **EKS** 방식을 결정했습니다.
- 또한 **Argo CD**를 연결하여 서비스 중단 없는 안정적인 배포 환경을 완성했습니다.
- 이번 발표를 통해 저희가 직접 부딪히고 겪었던 각 아키텍처의 장단점과, 최종적으로 현재의 인프라를 설계하기까지의 치열했던 고민 과정을 말씀드리겠습니다.

---

### 4. EC2 환경 — 첫 번째 시도

처음에는 S3 기반 프론트엔드와 로드밸런서, 오토스케일링을 적용한 EC2 백엔드를 구성했습니다.

그러나 가상 머신 기반의 아키텍처를 사용하며 아쉬운 점이 있었습니다.

**① 느린 스케일링 속도**
자동화된 환경을 구축했음에도 불구하고, 트래픽이 몰려 오토스케일링이 발동할 때 무거운 OS가 부팅되고 환경이 세팅되기까지 수 분의 시간이 소요되어 즉각적인 트래픽 대응이 어려웠습니다.

**② 개발/운영 환경 불일치**
개발 환경과 운영 환경의 불일치 시 OS 설정이나 Java 의존성 차이로 에러가 발생하는 경우도 있었습니다.

그래서 저희는 EC2 → ECS 환경으로 구성을 해봤습니다.

---

### 5. ECS 환경 — 두 번째 시도

이러한 EC2의 한계를 극복하기 위해, 저희는 컨테이너 기반의 아키텍처인 **ECS 환경**으로 인프라를 전면 개편했습니다.

ECS 환경에서도 로드밸런서와 오토스케일링을 동일하게 적용하여 프론트엔드와 백엔드를 새롭게 구성했고, 그 결과 인프라 관리 부담을 줄이고 스케일링 속도를 획기적으로 개선할 수 있었습니다.

그러나 ECS 역시 인프라 자동화 및 확장성 측면에서 아쉬운 점이 있었습니다.

**가장 큰 제약**: 글로벌 표준 컨테이너 오케스트레이션 시스템인 **쿠버네티스(Kubernetes)** 의 방대한 생태계를 활용할 수 없다는 점이었습니다. AWS 환경에 갇히게 되며, 배포 설정 파일을 오직 AWS에서만 사용할 수 있다는 확장성의 한계가 큰 발목을 잡을 것이라 생각했습니다.

---

### 6. EKS 환경 — 최종 선택

따라서 저희는 최종적으로 **EKS**를 사용하였습니다.

- 글로벌 표준인 쿠버네티스를 사용하며, 향후 어떤 환경으로 인프라를 확장하거나 이전하더라도 유연하게 대처할 수 있는 강력한 **이식성**을 확보했습니다.
- 기존 EC2, ECS의 로드밸런서와 오토스케일링을 넘어서, **파드 단위의 아주 빠르고 정교한 확장**을 가능케 하였습니다.
- 트래픽이 몰릴 때 컨테이너가 유연하게 늘어나며 안정적인 서비스를 유지할 수 있도록 하였습니다.

---

### 7. 일자별 결과물

- **FrontEnd/BackEnd 빌드스펙.yml** 및 각각의 파이프라인
- **EKS 배포 완료 후 상태 로그**
- **CloudWatch 연결 로그 그룹** — 오토스케일링 기본 세팅(최소 인스턴스 2개)
- **오토스케일링 시연** — 최소 2개 / 최대 4개 세팅 → 순간 트래픽 상승 시 4개로 늘어나는 모습
- **로드밸런서 시연** — ALB가 3개 가용영역(AZ)에 트래픽을 분산하는 로그
- **부하 테스트 결과** — 오토스케일링 그룹 2/2/2 세팅 기준, 300명이 순간적으로 5,000회를 호출하는 부하 테스트 및 CloudWatch 모니터링 결과

---

### 8. GitOps — ArgoCD 도입

저희는 GitOps, 자동화 운영 철학에 따라 최종 EKS 환경에 맞춰 **Argo CD**를 도입하였습니다.

**동작 방식**
개발자가 GitHub에 코드를 푸시하면 EKS 클러스터 내부에 설치된 Argo CD가 이를 감지하여 운영 중인 서버 상태와 깃허브에 올라온 코드 상태를 비교하며 **자동 동기화 및 무중단 배포**를 진행합니다.

**롤링 배포 전략**
버전을 변경하면 레플리카셋을 새로 생성하여, Pod 2개가 새로 생성되며 순차적으로 기존 Pod와 교체합니다. 이때 **롤링(Rolling) 방식**을 적용하여 무중단 배포를 구현했습니다.

**롤백**
직관적인 UI 형식이기에 장애 발생 시 **클릭 한 번으로 이전 버전으로 롤백** 할 수 있다는 강력하고 안정적인 운영 환경을 구성했습니다.

---

### 9. 트러블슈팅

#### ① EC2 오토스케일링 — 인스턴스 무한 재시작

**증상**: 인스턴스가 2개씩 생성되고 죽고를 반복. 서버 내부 스프링 부트는 문제없이 작동 중.

**원인**: 로드밸런서의 **상태 확인(Health Check) 경로 및 응답 설정 불일치**로, 서버의 상태를 확인하는 경로가 잘못되어 계속 에러코드를 반환하고 있었습니다. 유예기간 300초가 지나고 비정상으로 판단되어 강제 종료 → 재실행 과정을 반복하고 있었습니다.

**해결**: 상태 확인 경로를 올바르게 수정하여 2대의 인스턴스가 안정적으로 유지되는 것을 확인했습니다.

---

#### ② CloudFront + HTTP 백엔드 — 연결 거부

**증상**: 프론트엔드에 CloudFront를 적용 후, 백엔드와 재 연결하려 했으나 연결을 계속 거부.

**원인**: CloudFront는 **HTTPS** 보안을 적용하지만, 백엔드는 인증서 없는 **HTTP**로 배포되어 있었습니다. 암호화된 통신망(HTTPS)에서 HTTP로 접근 시도 시 발생하는 **Mixed Content 보안 차단**으로 연결할 수 없었습니다.

**해결**: ECS 환경 구성 시, 프론트엔드와 백엔드를 **동일한 도메인으로 묶어 통신을 일원화**하여 인증서 충돌 및 보안 취약점 문제를 해결했습니다.

---

### 10. 마무리

EC2 → ECS → EKS로 이어지는 인프라 발전 과정을 직접 경험하며, 단순히 기술 스택을 바꾸는 것이 아니라 각 환경의 근본적인 철학과 트레이드오프를 이해하는 귀중한 경험이었습니다. GitOps와 ArgoCD를 통한 자동화된 배포 파이프라인 구축, 그리고 실제 트러블슈팅 과정을 통해 운영 환경에서의 문제 해결 역량을 키울 수 있었습니다.

이상으로 6반 17조 AWS 미니프로젝트 발표를 마치겠습니다. 감사합니다.
