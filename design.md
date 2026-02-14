# Public Health RAG System
## Trustworthy AI Grounded in Official Health Sources

## 1. Problem Statement

Standard AI health chatbots suffer from critical limitations:
- **Hallucination**: Generate incorrect medical information
- **No Source Attribution**: Cannot verify information origin
- **Unsafe Responses**: Answer personal medical questions inappropriately
- **Outdated Knowledge**: Cannot update with latest health guidelines

## 2. Solution Overview

We propose a **Hybrid RAG-based Public Health Assistant** that:
- Indexes official documents (WHO, CDC, Government Health Agencies)
- Uses hybrid retrieval (semantic + keyword)
- Generates grounded responses only from retrieved documents
- Falls back safely when confidence is low

**Core Principle**: Accuracy and safety over completeness

## 3. System Architecture

### Two-Pipeline Design

**Offline Pipeline**: Document ingestion → Source validation → Chunking → Dual indexing (Vector + Keyword)


**Online Pipeline**: Query → Safety classification → Hybrid retrieval → Answer generation → Citation formatting

### Key Innovation: Hybrid Retrieval

**Semantic Search**: Captures conceptual meaning and handles paraphrased queries
**Keyword Search**: Preserves exact medical terminology and abbreviations
**Fusion**: Reciprocal Rank Fusion (RRF) combines both for optimal results

## 4. Core Components

### Document Ingestion
- **Trusted Sources Only**: WHO, CDC, Government Health Agencies
- **Validation**: Automatic source verification and rejection of unauthorized content
- **Processing**: Metadata extraction, text cleaning, semantic chunking (300-500 tokens)
- **Traceability**: Each chunk maintains source metadata for citations

### Safety Guardrails
Multi-layer query classification system:
- **Emergency Detection**: "Call 911" → Immediate emergency referral
- **Personal Medical**: "Do I have..." → Redirect to healthcare professionals
- **Out-of-Scope**: Low relevance → Conservative fallback

**Fallback Message**: "Please consult a licensed medical professional."

### Answer Generation
- **Context-Only**: LLM generates answers using only retrieved documents
- **Faithfulness Check**: Verify answer accuracy against source material
- **Citations**: Automatic source attribution with document titles, dates, and URLs

## 5. Technical Implementation

### Technology Stack
## Technology Stack

- **Backend**: FastAPI REST API  
- **Vector Search**: FAISS (Facebook AI Similarity Search) for dense embedding retrieval  
- **Keyword Search**: BM25 implementation for exact term matching  
- **LLM**: OpenAI GPT-4 API (or compatible LLM)  
- **Embeddings**: PubMedBERT (medical-domain optimized embeddings)  
- **Database**: PostgreSQL for document metadata and logging  

### Development Phases
1. **MVP**: Core RAG pipeline with basic safety guardrails
2. **Enhanced Safety**: Advanced faithfulness checking and query classification
3. **Production**: Scalability, monitoring, and comprehensive testing

## 6. Evaluation Metrics

To measure system performance and safety:

- **Faithfulness Score > 90%** (Answer grounded in retrieved context)
- **100% Citation Coverage** (Every response includes sources)
- **Retrieval Precision** on benchmark health queries
- **Response Time < 2 seconds**
- **Fallback Accuracy** (Correct detection of unsafe queries)

These metrics ensure both accuracy and safety are quantifiable.

## 7. Comparison with Standard AI Chatbots

| Feature | Standard LLM | Public Health RAG System |
|----------|--------------|--------------------------|
| Source Attribution | None | Mandatory |
| Hallucination Risk | High | Low |
| Out-of-Scope Handling | Attempts answer | Safe refusal |
| Knowledge Updates | Requires retraining | Add new documents |
| Transparency | Black box | Fully traceable |

## 8. Example Query Flow

**User Query:**  
"What are WHO guidelines for seasonal influenza prevention?"

System Steps:
1. Query passes safety classification
2. Hybrid retrieval fetches relevant WHO documents
3. RRF ranks top document chunks
4. LLM generates grounded summary
5. Citations attached from source metadata
6. Response returned with verifiable links

## 9. Innovation Highlights

- Hybrid Retrieval with Reciprocal Rank Fusion (RRF)
- Post-generation Faithfulness Verification
- Source-Restricted Knowledge Base
- Transparent Citation Mechanism


## 6. Expected Impact

### Safety Benefits
- **Zero Hallucination Goal**: All answers grounded in official sources
- **Transparent Attribution**: Users can verify every claim
- **Conservative Approach**: Refuses to answer rather than risk misinformation

### User Benefits
- **Trustworthy Information**: Only from WHO, CDC, and government agencies
- **Up-to-date Content**: Easy addition of new official publications
- **Clear Citations**: Direct links to source documents

### Technical Advantages
- **Hybrid Retrieval**: Best of semantic understanding and keyword precision
- **Scalable Architecture**: Cloud-ready design for institutional deployment
- **Audit Trail**: Complete logging for accountability and improvement

## 7. Conclusion

## 10. Conclusion

The Public Health RAG System demonstrates a practical and safety-focused approach to AI deployment in healthcare information systems. By combining hybrid retrieval, strict grounding, and conservative guardrails, the system minimizes hallucination risk while maintaining transparency and trust.

This architecture serves as a blueprint for safe AI systems in regulated domains and showcases how retrieval-augmented generation can be responsibly applied to help the common man.





