# Requirements Document: Public Health RAG System

---

## 1. Introduction

Generative AI systems frequently provide health-related responses that are hallucinated, uncited, or unreliable. This creates significant risk when users rely on AI-generated medical information.

The Public Health RAG System aims to address this problem by building a trustworthy AI assistant that:

- Uses only verified official health sources  
- Grounds every response in retrieved documents  
- Provides transparent citations  
- Refuses to answer when confidence is low  

This system is designed as an informational tool for general public health queries and does not replace professional medical advice.

---

## 2. Problem Statement

Existing AI-based health assistants suffer from:

- **Hallucination**: Generating unsupported or fabricated medical information  
- **Lack of traceability**: No source citations for verification  
- **Unsafe responses**: Providing personal medical advice  
- **Out-of-scope answering**: Responding beyond available knowledge  

These issues reduce trust and may lead to harmful decisions.

---

## 3. Objectives

1. Build a Retrieval-Augmented Generation (RAG) system using verified public health documents.
2. Implement hybrid retrieval combining semantic search (FAISS) and keyword search (BM25).
3. Ensure every answer includes proper source citations.
4. Prevent hallucination by enforcing context-only generation.
5. Provide measurable evaluation metrics for accuracy, faithfulness, and safety.
6. Design a modular architecture that supports scalability and future extensions.

---

## 4. Scope

### 4.1 In Scope

- Document ingestion from official public health sources (WHO, CDC, government health ministries)
- Semantic chunking of documents (200–500 tokens)
- Dense embedding generation for vector search
- FAISS-based semantic retrieval
- BM25-based keyword retrieval
- Reciprocal Rank Fusion (RRF) for result ranking
- Query safety classification
- Context-only LLM answer generation
- Automatic citation formatting
- Logging and monitoring
- REST API interface

### 4.2 Out of Scope

- Real-time medical diagnosis
- Personalized treatment recommendations
- Integration with hospital or EHR systems
- Prescription suggestions
- Emergency medical response systems
- Mobile application development
- Multi-modal inputs (images, audio, video)

---

## 5. Assumptions

- Users are seeking general public health information.
- Indexed documents are official and up-to-date.
- The LLM is used strictly for summarization of retrieved content.
- The system does not store personal health data.

---

## 6. Functional Requirements

### Requirement 1: Official Source Ingestion

The system SHALL ingest documents only from approved public health organizations.

Acceptance Criteria:

1. The system SHALL validate document sources against a predefined approved list.
2. The system SHALL reject unapproved sources.
3. The system SHALL extract metadata (title, source, publication date, URL).
4. The system SHALL log all ingestion events.

---

### Requirement 2: Document Processing and Chunking

The system SHALL preprocess and chunk documents for retrieval.

Acceptance Criteria:

1. Documents SHALL be split into semantically coherent chunks (200–500 tokens).
2. Chunk overlap SHALL be configurable.
3. Section headers SHALL be preserved.
4. Each chunk SHALL retain source metadata.

---

### Requirement 3: Semantic Retrieval (FAISS)

The system SHALL support embedding-based semantic search.

Acceptance Criteria:

1. Each document chunk SHALL be converted into a dense embedding vector.
2. Embeddings SHALL be stored in a FAISS index.
3. The system SHALL support approximate nearest neighbor search.
4. Semantic retrieval SHALL return top-k similar chunks.

---

### Requirement 4: Keyword Retrieval (BM25)

The system SHALL implement keyword-based retrieval using BM25.

Acceptance Criteria:

1. The system SHALL maintain a BM25 index over document chunks.
2. BM25 retrieval SHALL return ranked results based on keyword relevance.
3. The index SHALL be updated when new documents are ingested.
4. The system SHALL support exact medical term matching.

---

### Requirement 5: Hybrid Retrieval

The system SHALL combine semantic and keyword retrieval results.

Acceptance Criteria:

1. Both FAISS and BM25 retrieval SHALL execute for every query.
2. Top-k results from each method SHALL be retrieved independently.
3. Results SHALL be merged using Reciprocal Rank Fusion (RRF).
4. The system SHALL return the top-N ranked chunks for answer generation.

---

### Requirement 6: Safety Guardrails

The system SHALL prevent unsafe responses.

Acceptance Criteria:

1. The system SHALL detect emergency-related queries.
2. The system SHALL detect personal diagnosis or treatment requests.
3. The system SHALL refuse out-of-scope queries.
4. The fallback message SHALL be:
   "Please consult a licensed medical professional."

---

### Requirement 7: Context-Only Answer Generation

The system SHALL generate answers strictly from retrieved context.

Acceptance Criteria:

1. The LLM SHALL receive only retrieved document chunks.
2. The LLM SHALL be instructed not to use external knowledge.
3. If context is insufficient, the system SHALL return the fallback message.
4. Responses SHALL maintain neutral, informative tone.

---

### Requirement 8: Citation Generation

The system SHALL provide transparent source attribution.

Acceptance Criteria:

1. Citations SHALL include source name, title, publication date, and URL.
2. Citations SHALL be appended to each answer.
3. Duplicate citations SHALL be removed.
4. Inline citation numbering SHALL be supported.

---

### Requirement 9: Hallucination Prevention

The system SHALL verify answer faithfulness.

Acceptance Criteria:

1. Generated answers SHALL be checked against retrieved context.
2. If unsupported content is detected, the answer SHALL be rejected.
3. The system SHALL log rejected responses.
4. The system SHALL default to conservative refusal when uncertain.

---

### Requirement 10: API Interface

The system SHALL expose a REST API.

Acceptance Criteria:

1. Queries SHALL be submitted as JSON.
2. Responses SHALL include:
   - Answer text
   - Citations
   - Confidence score
   - Processing metadata
3. API endpoints SHALL be versioned.
4. API documentation SHALL include sample requests and responses.

---

## 7. Non-Functional Requirements

### 7.1 Accuracy

- High faithfulness score
- 100% citation coverage
- Strong retrieval precision

### 7.2 Performance

- Average query latency < 2 seconds
- Sub-second retrieval time
- Support for concurrent users

### 7.3 Scalability

- Modular design
- Support for incremental document ingestion
- Horizontal scaling capability

---

## 8. Safety Constraints

1. The system SHALL NOT provide personalized diagnosis.
2. The system SHALL NOT replace professional medical consultation.
3. The system SHALL refuse emergency medical queries.
4. The system SHALL default to conservative behavior when uncertain.

---

## 9. Evaluation Metrics

### Retrieval Metrics

- Precision@k
- Recall@k

### Faithfulness Metrics

- Faithfulness Score
- Hallucination Rate

### Citation Metrics

- Citation Coverage
- Citation Accuracy

### Performance Metrics

- Query Latency
- Retrieval Latency
- Generation Latency

### Safety Metrics

- Out-of-Scope Detection Rate
- False Rejection Rate
- Fallback Rate

---

## 10. Risk Analysis

### High-Risk Scenarios

- Hallucinated medical content  
- Outdated health guidelines  
- Misinterpreted user queries  

Mitigation: Hybrid retrieval, strict grounding, conservative fallback.

---

## 11. Future Enhancements

- Multilingual support
- Medical entity recognition
- Conversational memory
- Real-time document ingestion
- Confidence scoring improvements

---

## 12. Success Criteria

The system will be considered successful when:

1. Faithfulness score exceeds 90%.
2. All responses include citations.
3. Hallucination incidents are minimized.
4. Response times meet performance targets.
5. Users can verify all information via cited sources.

---

## 13. Conclusion

The Public Health RAG System establishes a safety-first architecture for trustworthy health information retrieval. By combining hybrid retrieval (FAISS + BM25), strict context-only generation, and transparent citations, the system minimizes hallucination risk while ensuring accountability and verifiability.

This requirements document defines a robust yet hackathon-feasible foundation for building a reliable AI-powered public health assistant.
