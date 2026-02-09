# KimHyeonJoo
김현주 포트폴리오 사이트

https://kkimhyeonjoo.github.io/KimHyeonJoo/

```mermaid
flowchart LR
  U[User Browser] -->|Upload images| FE[Streamlit UI]

  FE -->|Mode A Local pipelines| MONO[In App Pipelines]
  FE -->|Mode B MSA call base64| GW[Gateway API solve]

  subgraph MSA[Docker Compose Microservices]
    GW -->|subject literature| LIT[Literature Service process]
    GW -->|subject nonlit| NON[Non Literature Service process]
    GW -->|subject speech| SPC[Speech Writing Service process]
    GW -->|subject langmedia| LAM[Language Media Service process]
  end

  subgraph AI[AI Core]
    OCR[Vision OCR] --> POST[Post process symbols ranges]
    POST --> SOLVE[LLM solve and explain]
    SOLVE --> REC[Recommend similar problems]
  end

  subgraph DATA[Data Assets]
    VDB[(FAISS VectorStore)]
    EMB[Embeddings]
    BANK[(Problem Image Bank)]
    META[(Tags Metadata)]
    MODELS[(Local models dir)]
  end

  MONO --> OCR
  LIT --> OCR
  NON --> OCR
  SPC --> OCR
  LAM --> OCR

  SOLVE --> VDB
  VDB <-->|embed| EMB
  REC --> BANK
  REC --> META

  LIT --> MODELS
  NON --> MODELS
  SPC --> MODELS
  LAM --> MODELS

```


# 문학
```
flowchart TB
  IN[Input passage image and question image] --> QOCR[Question OCR]
  IN --> POCR[Passage OCR]

  POCR --> SPLIT[Split image OCR]
  SPLIT --> SYM[Restore symbols underline]
  SYM --> RANGE[Insert section markers A B]
  RANGE --> PTXT[Clean passage text]

  QOCR --> QTXT[Question text]
  PTXT --> CLASS[Classify query concept or problem]
  QTXT --> CLASS

  CLASS -->|problem| RET[Retriever MMR]
  RET --> VDB[(FAISS)]
  VDB --> REF[Reference context]
  REF --> GEN[LLM answer and explanation]

  CLASS -->|concept| GEN2[LLM concept answer]

  GEN --> TAG[Generate tags JSON]
  TAG --> HYB[Hybrid scoring tags and embeddings]
  HYB --> OUT[Output answer and similar problems topK]

```
