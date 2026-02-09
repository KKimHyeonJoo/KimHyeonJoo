# KimHyeonJoo
김현주 포트폴리오 사이트

https://kkimhyeonjoo.github.io/KimHyeonJoo/

```mermaid
graph TD
    %% User Input Stage
    A[User Image Input: Passage & Question] --> B{Streamlit App UI}
    
    %% Task Orchestration
    B --> C{Subject Router}
    
    %% Individual Subject Pipelines
    C -->|Literature| D[Literature Pipeline]
    C -->|Non-Lit/Speech/Media| E[Other Pipelines]
    
    %% Literature Pipeline Detail
    subgraph Literature_Pipeline_Logic [Literature Pipeline Detail]
        D1[OCR: Extract Text] --> D2[Text Refinement & Structuring]
        D2 --> D3[RAG: FAISS Vector Store Search]
        D3 --> D4[LLM: Answer & Explanation Generation]
        D2 --> D5[Hybrid Recommendation System]
    end
    
    %% Output Stage
    D4 --> F[Display Results]
    D5 -->|Tag + Embedding Similarity| F
```
