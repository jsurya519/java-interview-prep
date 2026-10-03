# Production-Ready Enterprise RAG — Interview Design

## 1. What did we build?

I designed a production-oriented enterprise knowledge assistant using:

* Java
* Spring Boot
* Spring AI
* Apache Kafka
* MySQL
* Vector Store
* LLM
* Docker
* Kubernetes

The system has two major flows:

### Document ingestion flow

```text
PDF
 ↓
Object Storage
 ↓
API
 ↓
Kafka
 ↓
Worker
 ↓
Parse / OCR
 ↓
Normalize
 ↓
Chunk
 ↓
Metadata
 ↓
Embedding
 ↓
Vector Store
 ↓
COMPLETED
```

### Query flow

```text
User Question
 ↓
Query Processing
 ↓
Semantic Vector Search
 +
Keyword Search
 +
Metadata Filtering
 ↓
Reranking
 ↓
Context Selection
 ↓
Prompt Construction
 ↓
LLM
 ↓
Grounded Answer + Source Citation
```

The important architectural idea is:

> **Document ingestion is asynchronous, while query processing is request-driven.**

---

# 2. Document Upload — First API Layer

The UI allows the user to upload a PDF.

The actual PDF is stored in **cloud/object storage**, rather than being sent through Kafka.

For example:

```text
UI
 ↓
Object Storage
 ↓
employee-policy.pdf
```

The object storage gives us a location/reference for the document.

The UI then calls our Spring Boot API with a JSON request containing the document location.

Example:

```json
{
  "documentUrl": "https://storage/.../employee-policy.pdf"
}
```

The API is responsible for accepting the ingestion request and putting the work onto Kafka.

The API should not wait for the complete document processing.

It should respond quickly with something like:

```json
{
  "documentId": "doc-123",
  "status": "QUEUED"
}
```

The user can use the ID to track the processing status.

---

# 3. Why don't we put the PDF into Kafka?

Kafka carries the **event/request**, not the actual PDF.

Kafka message:

```json
{
  "documentId": "doc-123",
  "documentUrl": "https://storage/.../employee-policy.pdf"
}
```

Object storage:

```text
employee-policy.pdf
```

So:

```text
Object Storage
    ↓
actual PDF

Kafka
    ↓
small JSON event telling the worker
which document needs processing
```

This keeps Kafka focused on asynchronous event/message processing.

---

# 4. SHA-256 Deduplication

The SHA-256 hash is calculated from the **actual PDF file bytes**.

It is not calculated from extracted text.

Conceptually:

```text
PDF
 ↓
Read file bytes
 ↓
SHA-256
 ↓
Hash
```

Example:

```text
employee-policy.pdf
        ↓
SHA-256
        ↓
a8f3c9...7b21
```

The purpose is document-level deduplication.

If the exact same PDF is uploaded again:

```text
Same PDF bytes
      ↓
Same SHA-256
      ↓
Document already exists
      ↓
Don't process it again
```

This is done **before expensive processing such as parsing, chunking and embedding**.

---

# 5. Where is SHA-256 calculated?

There are two possible implementations depending on where the PDF bytes are available.

For our design, the important principle is:

> **Calculate the document fingerprint before expensive processing.**

If the worker retrieves the PDF from object storage, the worker can calculate the SHA-256 before beginning parsing.

The flow is:

```text
Kafka message
 ↓
Worker
 ↓
Download PDF from Object Storage
 ↓
SHA-256
 ↓
Check MySQL
 ↓
Duplicate?
 ├── YES → Skip processing
 └── NO  → Continue
```

---

# 6. MySQL Database Design

For the basic design, we have three logical areas:

```text
documents
document_metadata
document_chunks
```

And optionally, if we want detailed stage-level processing tracking, we can have:

```text
document_processing
```

But the core design can work with the first three.

---

# 7. `documents` Table

This is the main document table.

It represents the document and its overall lifecycle.

Example schema:

```sql
CREATE TABLE documents (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    file_name VARCHAR(255) NOT NULL,
    storage_url VARCHAR(1000) NOT NULL,
    sha256_hash VARCHAR(64) NOT NULL UNIQUE,
    status VARCHAR(30) NOT NULL,
    retry_count INT DEFAULT 0,
    error_message VARCHAR(2000),
    version INT DEFAULT 1,
    created_at TIMESTAMP,
    updated_at TIMESTAMP
);
```

Important fields:

### `id`

Unique document identifier.

Example:

```text
101
```

### `storage_url`

Where the original PDF exists.

Example:

```text
https://storage/.../employee-policy.pdf
```

### `sha256_hash`

Used for deduplication.

```text
a8f3c9...7b21
```

The `UNIQUE` constraint prevents duplicate documents with the same hash.

### `status`

Represents the document's lifecycle.

```text
QUEUED
PROCESSING
COMPLETED
FAILED
```

### `retry_count`

Number of processing retries.

Example:

```text
0
1
2
3
```

### `error_message`

Stores the latest processing failure when applicable.

---

# 8. Document Lifecycle

The document moves through states:

```text
QUEUED
   ↓
PROCESSING
   ↓
COMPLETED
```

If a transient failure occurs:

```text
PROCESSING
   ↓
Failure
   ↓
retry_count++
   ↓
QUEUED
   ↓
Kafka
   ↓
Worker
```

If the maximum retry limit is reached:

```text
PROCESSING
   ↓
Failure
   ↓
retry_count >= max
   ↓
FAILED
```

A permanent problem, such as an invalid/corrupted document, should not be retried indefinitely.

---

# 9. `document_metadata` Table

This table stores information about the document itself.

Example:

```sql
CREATE TABLE document_metadata (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    document_id BIGINT NOT NULL,
    title VARCHAR(500),
    author VARCHAR(255),
    department VARCHAR(255),
    document_type VARCHAR(100),
    created_date DATE,
    FOREIGN KEY (document_id) REFERENCES documents(id)
);
```

This is **document-level metadata**.

For example:

```text
document_id = 101
title       = Employee Leave Policy
department  = HR
type        = POLICY
```

It is useful later for metadata filtering.

For example:

```text
department = HR
```

during retrieval.

---

# 10. `document_chunks` Table

A single PDF can generate many chunks.

Therefore this is a **one-to-many relationship**.

```text
One document
      ↓
Many chunks
```

Example schema:

```sql
CREATE TABLE document_chunks (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    document_id BIGINT NOT NULL,
    chunk_index INT NOT NULL,
    content TEXT NOT NULL,
    page_number INT,
    created_at TIMESTAMP,
    FOREIGN KEY (document_id) REFERENCES documents(id)
);
```

Suppose one PDF produces 100 chunks.

Then:

```text
documents
    |
    | document_id = 101
    |
    +---- chunk 1
    +---- chunk 2
    +---- chunk 3
    ...
    +---- chunk 100
```

So there will be approximately **100 rows in `document_chunks`** for that document.

---

# 11. Document Metadata vs Chunk Metadata

This distinction is important.

### Document metadata

One document:

```text
document_metadata
-------------------------
document_id
title
author
department
document_type
```

### Chunk information

Many rows:

```text
document_chunks
-------------------------
chunk_id
document_id
chunk_index
content
page_number
```

Therefore:

```text
documents
   |
   +---- document_metadata       1 row
   |
   +---- document_chunks         many rows
```

---

# 12. Kafka

Once the document ingestion request is accepted, the API publishes a message to Kafka.

Example:

```json
{
  "documentId": "101",
  "documentUrl": "https://storage/.../employee-policy.pdf"
}
```

The Kafka topic might conceptually be:

```text
document-ingestion
```

Kafka is responsible for decoupling the API from the processing workers.

The API does not call the worker directly.

Instead:

```text
API
 ↓
Kafka
 ↓
Worker
```

---

# 13. Kafka Consumer / Worker

The worker is a separate service/process.

It continuously consumes available messages from Kafka.

It is not bound to the API.

Conceptually:

```text
Kafka
 ↓
Consumer
 ↓
Worker
```

The worker receives:

```json
{
  "documentId": "101",
  "documentUrl": "https://storage/.../employee-policy.pdf"
}
```

Then it retrieves the PDF from object storage.

```text
Kafka message
      ↓
Worker
      ↓
Object Storage
      ↓
PDF
```

---

# 14. Kafka Batching vs Multiple Consumers

Kafka consumers can poll multiple records rather than processing only one record at a time.

For example:

```text
Kafka
 ↓
Consumer
 ↓
[message1, message2, message3, ...]
```

This is batching.

Multiple consumers/workers can also process partitions in parallel.

These are different concepts:

> **Batching = processing multiple records together.**

> **Multiple consumers/partitions = parallel processing.**

For the interview, don't overcomplicate this unless asked.

---

# 15. PDF Parsing

The worker now has the actual PDF.

First we extract its content.

```text
PDF
 ↓
PDF Parser
 ↓
Text / Structure
```

We want to preserve useful information such as:

```text
text
page number
headings
paragraphs
tables where possible
```

---

# 16. OCR

Some PDFs are scanned images rather than real text.

For those documents:

```text
PDF
 ↓
No usable text
 ↓
OCR
 ↓
Extracted text
```

So the worker can handle both:

```text
Normal PDF → Parsing

Scanned PDF → OCR
```

This is part of the ingestion pipeline in the resume.

---

# 17. Document Normalization

After parsing/OCR, the extracted content may have formatting problems:

```text
extra spaces
unnecessary line breaks
OCR artifacts
repeated headers/footers
encoding inconsistencies
```

Normalization cleans this up.

Example:

```text
EMPLOYEE   BENEFITS


Health     Insurance

Employees are eligible
for    health insurance
```

becomes:

```text
EMPLOYEE BENEFITS

Health Insurance

Employees are eligible for health insurance
```

The goal is:

> **Create a clean and consistent representation without changing the meaning.**

---

# 18. Chunking

After normalization, we split the document into smaller pieces.

Why?

Because we don't want to create one enormous embedding for the entire PDF.

Example:

```text
PDF
 ↓
Normalized text
 ↓
Chunking
 ↓
Chunk 1
Chunk 2
Chunk 3
...
Chunk 100
```

We can use configurable chunk size and overlap.

Example:

```text
Chunk size = 500 tokens
Overlap = 50 tokens
```

The overlap helps preserve context between boundaries.

Each chunk has metadata such as:

```text
chunk_id
document_id
chunk_index
page_number
content
```

---

# 19. Embedding Generation

Now each chunk is converted into a vector.

```text
Chunk text
   ↓
Embedding Model
   ↓
Vector
```

Example:

```text
Chunk 1
 ↓
[0.12, 0.81, -0.21, ...]
```

For 100 chunks:

```text
Chunk 1   → Vector 1
Chunk 2   → Vector 2
...
Chunk 100 → Vector 100
```

These vectors are stored in the vector store.

---

# 20. Vector Store and MySQL Relationship

We maintain a stable `chunk_id`.

Example MySQL:

```text
document_chunks

chunk_id | document_id | chunk_index | page
---------|-------------|-------------|-----
C101     | D10         | 1           | 1
C102     | D10         | 2           | 1
C103     | D10         | 3           | 2
```

Vector store:

```text
vector_id | chunk_id | embedding
----------|----------|----------
V001      | C101     | [...]
V002      | C102     | [...]
V003      | C103     | [...]
```

The `chunk_id` is the bridge.

```text
Vector Store
     |
     | chunk_id = C102
     ↓
MySQL
     |
     ↓
document_chunks
     |
     ↓
document_id / page / content
```

The vector store can also keep useful metadata alongside the vector.

---

# 21. Vector Indexing

Once embeddings are generated, they are inserted into the vector store.

Conceptually:

```text
Chunk
 ↓
Embedding
 ↓
Vector Store
```

The vector store allows us to perform similarity search later.

At the end of successful ingestion:

```text
documents.status = COMPLETED
```

---

# 22. Complete Ingestion Flow

The entire flow is:

```text
                    UI
                     |
                     | Upload PDF
                     ↓
              Object Storage
                     |
                     | document URL
                     ↓
              Spring Boot API
                     |
                     | JSON request
                     ↓
                   Kafka
                     |
                     | consume
                     ↓
                Worker Service
                     |
                     ↓
              Retrieve PDF
                     |
                     ↓
                SHA-256
                     |
              ┌──────┴──────┐
              |             |
          Duplicate       New
              |             |
             Stop           ↓
                       PDF Parsing
                            ↓
                           OCR
                            ↓
                      Normalization
                            ↓
                         Chunking
                            ↓
                    Metadata Extraction
                            ↓
                    Embedding Generation
                            ↓
                     Vector Indexing
                            ↓
                       COMPLETED
```

---

# 23. Query Flow — User Asks a Question

Now the document is already indexed.

The user asks:

> "What is the parental leave duration?"

The request comes to our Spring Boot API.

```text
User
 ↓
Spring Boot API
 ↓
RAG / Query Orchestration
```

---

# 24. Query Processing

The system processes the user question.

For multi-turn conversations, previous conversation context can help understand the current question.

For example:

```text
User:
"What is our parental leave policy?"

Assistant:
...

User:
"How long can fathers take?"
```

The second question depends on the previous context.

The query-processing layer can use that context to understand the user's intent.

---

# 25. Semantic Vector Search

The user query is converted into an embedding.

```text
"What is the parental leave duration?"
              ↓
       Embedding Model
              ↓
         Query Vector
```

We then search the vector store for semantically similar chunks.

Example:

```text
Chunk A → 0.91
Chunk B → 0.88
Chunk C → 0.84
Chunk D → 0.80
```

These are candidate chunks.

---

# 26. Keyword Search

Semantic search isn't always enough.

For example, if the user asks:

> "What does policy HR-4827 say?"

A keyword search can be useful because the exact identifier may exist in the document.

So we combine:

```text
Semantic Vector Search
          +
Keyword Search
```

This is called **hybrid retrieval**.

Conceptually:

```text
                 User Query
                     |
          ┌──────────┴──────────┐
          ↓                     ↓
    Vector Search        Keyword Search
          |                     |
          └──────────┬──────────┘
                     ↓
              Candidate Chunks
```

---

# 27. Metadata Filtering

We can also filter documents based on metadata.

For example:

```text
department = HR
document_type = POLICY
```

This prevents irrelevant documents from being considered.

It is also important for enterprise retrieval because we should only retrieve documents within the allowed scope.

So the retrieval stage combines:

```text
Semantic Search
+
Keyword Search
+
Metadata Filtering
```

---

# 28. Reranking

The initial retrieval gives us candidate chunks.

Example:

```text
Chunk A → 0.91
Chunk B → 0.88
Chunk C → 0.85
Chunk D → 0.82
```

These scores represent the initial retrieval relevance/similarity.

A reranker can take:

```text
Query + Candidate Chunk
```

and calculate a more focused relevance score.

For example:

```text
Query + A → 0.72
Query + B → 0.95
Query + C → 0.81
Query + D → 0.43
```

The new order becomes:

```text
B
C
A
D
```

The top-ranked chunks are then selected.

A dedicated reranking model is a separate model from the embedding model.

Its job is:

> **Given a query and candidate text, determine how relevant that candidate is to the query.**

---

# 29. Context Selection

We don't send every retrieved chunk to the LLM.

Suppose retrieval produces 20 candidates.

After reranking:

```text
20 candidates
     ↓
Top 10
     ↓
Context selection
     ↓
Top 5
     ↓
LLM
```

Context selection considers the available context/token limit and chooses the most useful chunks.

The objective is:

> **Give the LLM high-quality, relevant context rather than a large amount of noisy information.**

---

# 30. Prompt Construction

Now we have:

```text
User question
+
Conversation context
+
Retrieved document chunks
```

We construct the prompt.

Conceptually:

```text
System instructions

Use the supplied context to answer.
Do not invent information.
If the context is insufficient, say so.

Context:

Source 1:
Employee Leave Policy
Page 15
"..."

Source 2:
HR Policy
Page 4
"..."

Question:
What is the parental leave duration?
```

This prompt is sent to the LLM.

---

# 31. LLM Orchestration with Spring AI

Spring AI acts as the abstraction/orchestration layer for model interaction.

The application doesn't need to tightly couple every component directly to a particular model.

The orchestration layer handles:

```text
Query processing
Conversation context
Prompt construction
LLM invocation
Embedding invocation
Response handling
```

The project supports configurable model providers, including local Ollama-based inference and interchangeable foundation models.

Conceptually:

```text
Application
     ↓
Spring AI
     ↓
LLM Provider
```

---

# 32. Grounded Answer

The LLM receives the retrieved context and generates an answer based on that context.

Example:

```text
Question:
How long is parental leave?

Retrieved context:
Employees are eligible for up to 12 weeks...
```

LLM response:

```text
Eligible employees can take up to 12 weeks
of parental leave.
```

The important concept is:

> **The LLM is generating the answer from retrieved enterprise information rather than relying only on its pretrained knowledge.**

That's the "RAG" part.

---

# 33. Source-Level Citations

During chunking, we preserve source information:

```text
chunk_id
document_id
page_number
document name
```

When retrieval returns a chunk, we still know where that chunk came from.

Example:

```text
Chunk C102
document_id = D10
page = 15
```

So the application can provide:

```text
Answer:
Employees can take up to 12 weeks of parental leave.

Source:
Employee Leave Policy — Page 15
```

The stable `chunk_id` connects:

```text
Vector Store
      ↓
chunk_id
      ↓
MySQL / document metadata
      ↓
Original document
      ↓
Citation
```

---

# 34. Reliability — Retries

Retries are for **temporary/transient failures**.

For example:

```text
Worker
 ↓
Embedding Service
 ↓
Network failure
```

Instead of immediately failing:

```text
Retry 1
 ↓
Retry 2
 ↓
Retry 3
```

If the operation succeeds:

```text
Continue processing
```

If it keeps failing:

```text
Mark failed / requeue according to retry policy
```

Retries can be useful for:

* LLM
* Embedding service
* Vector store
* Network calls

Important:

> **We should not retry permanent failures indefinitely.**

---

# 35. Timeouts

A dependency might simply stop responding.

For example:

```text
Worker
 ↓
Embedding Model
 ↓
waiting...
```

Without a timeout, the worker could wait indefinitely.

With a timeout:

```text
Request
 ↓
10 seconds
 ↓
TIMEOUT
```

Then we can retry, fallback, or fail according to the situation.

Timeouts can protect calls to:

* LLM
* Embedding model
* Vector store

Simple definition:

> **A timeout prevents our application from waiting indefinitely for a dependency.**

---

# 36. Circuit Breaker

Suppose the LLM service is down.

Without a circuit breaker:

```text
Request 1 → LLM → failure
Request 2 → LLM → failure
Request 3 → LLM → failure
...
Request 100 → LLM → failure
```

We keep sending requests to an unhealthy dependency.

Circuit breaker:

```text
Repeated failures
       ↓
Circuit OPEN
       ↓
Stop calling dependency
       ↓
Fail fast / fallback
```

After some time, the circuit can test whether the dependency has recovered.

The natural dependencies for circuit breakers in this project are:

```text
LLM
Embedding Service
Vector Store
```

---

# 37. Graceful Fallbacks

Fallback means providing a controlled alternative when a dependency isn't available.

For example:

```text
User Query
 ↓
Retrieval works
 ↓
LLM unavailable
```

Instead of an uncontrolled failure, the system can return a controlled message:

> "The relevant information was retrieved, but the answer-generation service is temporarily unavailable. Please try again."

Another example in hybrid retrieval:

```text
Vector Search unavailable
        ↓
Keyword Search still available
        ↓
Retrieve using keyword search
```

The exact fallback depends on the dependency and what functionality can safely continue.

---

# 38. Caching

Caching is a performance optimization.

The simple idea:

> **If we already have the result of an expensive operation, reuse it instead of making the same expensive call again.**

For example:

```text
User Query
 ↓
Cache
 ├── HIT  → return cached result
 │
 └── MISS
       ↓
     RAG pipeline
       ↓
     LLM
       ↓
     Answer
       ↓
     Cache
```

Caching can reduce:

* repeated LLM calls
* repeated retrieval work
* latency
* model usage

The cache must be designed carefully around things such as user access and document changes.

---

# 39. Batching

A single document can generate many chunks.

For example:

```text
PDF
 ↓
100 chunks
```

Instead of making 100 separate downstream calls:

```text
Chunk 1 → embedding request
Chunk 2 → embedding request
...
Chunk 100 → embedding request
```

we can batch:

```text
100 chunks
 ↓
Batch 1
Batch 2
Batch 3
...
 ↓
Embedding Model
```

We can also batch vector-store writes.

Benefits:

* Higher throughput
* Less network overhead
* Better utilization of downstream services

Kafka consumers can also poll multiple records, but:

> **Batching and parallel consumers are different concepts.**

Batching = process multiple records/items together.

Multiple consumers/partitions = parallel processing.

---

# 40. Observability

The system needs visibility into what is happening.

We collect metrics such as:

### Ingestion

```text
documents processed
ingestion throughput
processing failures
```

### Retrieval

```text
retrieval latency
reranking latency
```

### LLM

```text
LLM latency
token usage
LLM failures
```

### End-to-end

```text
total request latency
```

For example:

```text
End-to-end latency = 2.5 sec

Query processing = 50 ms
Retrieval = 150 ms
Reranking = 300 ms
LLM = 2 sec
```

This helps identify where the bottleneck is.

---

# 41. Structured Observability

Instead of random log messages, logs should contain structured information.

For example:

```json
{
  "requestId": "req-123",
  "documentId": "doc-101",
  "operation": "embedding",
  "latencyMs": 250,
  "status": "SUCCESS"
}
```

This makes troubleshooting and monitoring easier.

---

# 42. Evaluation Framework

A production RAG system needs more than:

> "It seems to answer correctly."

We create an evaluation dataset containing questions and expected/relevant information.

Then we can evaluate:

```text
Retrieval relevance
Context quality
Groundedness
Answer quality
```

We can compare different configurations.

For example:

```text
Chunking strategy A
       vs
Chunking strategy B
```

or:

```text
Retrieval configuration A
       vs
Retrieval configuration B
```

The goal is to systematically understand whether a change improves RAG quality.

---

# 43. Docker and Kubernetes

The platform is containerized using Docker.

Conceptually:

```text
Spring Boot API
      ↓
Docker Container

Worker
      ↓
Docker Container
```

Kubernetes can then deploy and scale these workloads.

The important point from the resume is that the workloads can be independently scalable:

```text
Kubernetes
    |
    +---- API workload
    |
    +---- Ingestion/worker workload
```

If query traffic increases, API capacity can be scaled.

If document ingestion increases, worker capacity can be scaled independently.

---

# 44. Complete Architecture

Put everything together:

```text
                         ┌──────────────┐
                         │      UI      │
                         └──────┬───────┘
                                │
                         Upload PDF
                                │
                                ▼
                       ┌─────────────────┐
                       │ Object Storage  │
                       │                 │
                       │ Actual PDF      │
                       └────────┬────────┘
                                │
                         Document URL
                                │
                                ▼
                       ┌─────────────────┐
                       │ Spring Boot API │
                       └────────┬────────┘
                                │
                         JSON Event
                                │
                                ▼
                       ┌─────────────────┐
                       │     Kafka       │
                       └────────┬────────┘
                                │
                           Consume
                                │
                                ▼
                       ┌─────────────────┐
                       │ Ingestion       │
                       │ Worker          │
                       └────────┬────────┘
                                │
                         Retrieve PDF
                                │
                                ▼
                            SHA-256
                                │
                         Deduplication
                                │
                                ▼
                       PDF Parse / OCR
                                │
                                ▼
                         Normalization
                                │
                                ▼
                           Chunking
                                │
                                ├───────────────┐
                                ▼               ▼
                         MySQL Metadata   Embedding Model
                                                │
                                                ▼
                                          Vector Store
```

Query side:

```text
                    User Question
                         │
                         ▼
                 Spring Boot API
                         │
                         ▼
                 Query Processing
                         │
                ┌────────┴────────┐
                ▼                 ▼
         Vector Search       Keyword Search
                │                 │
                └────────┬────────┘
                         ▼
                 Metadata Filtering
                         │
                         ▼
                     Candidates
                         │
                         ▼
                     Reranking
                         │
                         ▼
                 Context Selection
                         │
                         ▼
                Prompt Construction
                         │
                         ▼
                    Spring AI
                         │
                         ▼
                       LLM
                         │
                         ▼
              Grounded Answer
                         │
                         ▼
                 Source Citations
```

---

# 45. The Entire Project in One Interview Answer

If the interviewer asks:

> **"Explain your RAG project end to end."**

A strong answer is:

> "I designed an enterprise knowledge assistant using Java, Spring Boot and Spring AI with a RAG architecture. The system has an asynchronous document ingestion flow and a synchronous query flow.
>
> For ingestion, the user uploads a PDF to object storage and the client sends the document location to our Spring Boot API. The API publishes a lightweight JSON ingestion event containing the document information to Kafka and immediately returns a document ID with a queued status. Separate worker services consume these Kafka messages asynchronously.
>
> The worker retrieves the PDF from object storage and calculates its SHA-256 fingerprint for document-level deduplication. We maintain document metadata and lifecycle information in MySQL, including processing status and retry information. New documents go through PDF parsing, OCR where required, normalization, metadata extraction and chunking. Each chunk is stored with metadata such as document ID and page information. We generate embeddings for the chunks and index them in the vector store.
>
> For querying, the user's question goes through query processing and then a multi-stage retrieval pipeline. We combine semantic vector search, keyword-based retrieval and metadata filtering to obtain candidate chunks. We then rerank those candidates and select the most relevant context within our context limit.
>
> The selected context, together with the user question and conversation context, is passed through our Spring AI orchestration layer to the LLM. The LLM generates a grounded response based on the retrieved enterprise documents. Because each chunk retains its document and page metadata, we can provide source-level citations with the response.
>
> For production reliability, we use caching and batching for performance, timeouts and retries for transient dependency failures, circuit breakers to protect against repeatedly failing LLM, embedding and vector-store dependencies, and graceful fallbacks where possible. We also collect metrics and structured observability around ingestion throughput, retrieval latency, LLM latency, token usage, failures and end-to-end latency.
>
> Finally, we have an evaluation framework to measure retrieval relevance, context quality, groundedness and answer quality. The platform is containerized with Docker and designed for Kubernetes deployment so API and worker workloads can scale independently."

---

# 46. The Mental Model to Remember Before the Interview

If you forget everything else, remember this:

```text
                INGESTION
                   │
PDF → Storage → API → Kafka → Worker
                              │
                              ▼
                    Parse / OCR
                              ↓
                         Normalize
                              ↓
                         Chunk
                              ↓
                       Embeddings
                              ↓
                       Vector Store


                 QUERY / RAG
                   │
User → API → Query Processing
                   ↓
          Vector + Keyword Search
                   ↓
          Metadata Filtering
                   ↓
               Reranking
                   ↓
           Context Selection
                   ↓
          Prompt Construction
                   ↓
                 LLM
                   ↓
        Grounded Answer + Citation
```

And the supporting pieces are:

```text
MySQL
→ document metadata
→ lifecycle/status
→ retry information
→ chunk metadata

Kafka
→ asynchronous ingestion events

Object Storage
→ actual PDF

Vector Store
→ embeddings + retrieval

Spring AI
→ LLM / embedding orchestration

Retries
→ temporary failures

Timeouts
→ don't wait forever

Circuit Breakers
→ protect unhealthy dependencies

Fallbacks
→ controlled behavior when dependencies fail

Caching
→ avoid repeated expensive work

Batching
→ improve throughput

Observability
→ know where the system is slow/failing

Evaluation
→ know whether RAG quality is actually improving

Docker/Kubernetes
→ deployment and independent scaling
```

## The one sentence that ties the whole project together

> **"The core idea is to asynchronously transform enterprise documents into searchable vector representations, retrieve the most relevant and authorized context for each user query, and use that context to generate grounded answers with source citations, while adding the reliability, observability and scalability required for production."**
