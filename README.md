# KimHyeonJoo
김현주 포트폴리오 사이트

https://kkimhyeonjoo.github.io/KimHyeonJoo/

```mermaid
flowchart LR
  U[사용자/브라우저] -->|지문·문항 이미지 업로드| FE[Streamlit Frontend]

  %% 실행 방식 2가지(통합 UI 기준)
  FE -->|옵션 A: 로컬/모놀리식 실행| MONO[In-App Pipelines]
  FE -->|옵션 B: MSA 호출(이미지 base64)| GW[Gateway API /solve]

  %% MSA 브랜치
  subgraph MSA[Docker Compose / Microservices]
    GW -->|subject=문학| LIT[Subject Service: Literature /process]
    GW -->|subject=비문학| NON[Subject Service: Non-Literature /process]
    GW -->|subject=화작| SPC[Subject Service: Speech&Comp /process]
    GW -->|subject=언매| LAM[Subject Service: Lang&Media /process]
  end

  %% 공통 AI 컴포넌트(과목 내부에서 사용)
  subgraph AI[AI Core Components]
    OCR[Vision OCR (LLM Vision)] --> POST[후처리/정교화\n(기호·밑줄·구간 복원)]
    POST --> SOLVE[정답·해설 생성 (LLM)]
    SOLVE --> REC[유사문제 추천]
  end

  %% 데이터/리소스
  subgraph DATA[Data / Assets]
    VDB[(FAISS VectorStore)]
    EMB[Embedding Model]
    BANK[(문항/지문 이미지 뱅크)]
    META[(태그/메타데이터)]
    MODELS[(로컬 models/ 폴더)]
  end

  %% 연결
  MONO --> OCR
  LIT --> OCR
  NON --> OCR
  SPC --> OCR
  LAM --> OCR

  SOLVE --> VDB
  VDB <-->|임베딩| EMB
  REC --> BANK
  REC --> META
  LIT --> MODELS
  NON --> MODELS
  SPC --> MODELS
  LAM --> MODELS
```

```
flowchart TB
  IN[입력: 지문 이미지 + 문항 이미지] --> QOCR[문항 OCR]
  IN --> POCR[지문 OCR]

  %% 지문 OCR 정교화(문학 핵심)
  POCR --> SPLIT[이미지 분할 OCR]
  SPLIT --> SYM[특수기호/밑줄 복원]
  SYM --> RANGE[[A]/[B] 구간 삽입]
  RANGE --> PTXT[정교화된 지문 텍스트]

  %% RAG 기반 해설
  QOCR --> QTXT[문항 텍스트]
  PTXT --> CLASS[질문 유형 분기\n(개념 vs 문제)]
  QTXT --> CLASS

  CLASS -->|문제| RET[Retriever(MMR)]
  RET --> VDB[(FAISS)]
  VDB --> REF[Reference Context]
  REF --> GEN[정답·해설 생성(LLM)]
  CLASS -->|개념| GEN2[개념 답변 생성(LLM)]

  %% 추천(하이브리드)
  GEN --> TAG[문항 태깅(JSON)]
  TAG --> HYB[Hybrid Scoring\n(태그 유사도 + 임베딩)]
  HYB --> OUT[출력: 정답·해설 + 유사문제 Top-K\n(지문/문항 이미지 경로 포함)]

```
