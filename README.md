# HDFC Bank: Custom LLM Development Pipeline

**An AI-powered conversational banking assistant using Fine-Tuned LLMs, Retrieval-Augmented Generation (RAG), and SQL-based data retrieval.**

![Python](https://img.shields.io/badge/Python-3.12-blue)
![FastAPI](https://img.shields.io/badge/Backend-FastAPI-green)
![PostgreSQL](https://img.shields.io/badge/Database-PostgreSQL-blue)
![LLM](https://img.shields.io/badge/LLM-Qwen2.5-orange)
![RAG](https://img.shields.io/badge/GenAI-RAG-purple)

---

## 1. Project Overview

The HDFC Banking AI Assistant is an Applied Generative AI project designed to demonstrate how Large Language Models (LLMs), Retrieval-Augmented Generation (RAG), and structured database queries can work together to provide a conversational banking experience.

Users can interact with the assistant through natural language to retrieve account information, check transaction details, and ask banking-related questions.

The application combines a fine-tuned Qwen model, a hybrid RAG retrieval pipeline, and a FastAPI backend to process user requests and generate relevant responses.

### Project Objectives

- Build an AI-powered conversational banking assistant.
- Use a fine-tuned LLM for intent understanding and response generation.
- Implement RAG for knowledge-grounded banking answers.
- Enable authenticated access to customer-specific banking information.
- Integrate SQL-based data retrieval with natural-language interaction.
- Implement a modular backend architecture using FastAPI.

**Important:** This is an educational prototype using synthetic banking data. It is not connected to HDFC Bank's live banking infrastructure or real customer accounts.

---

## 2. Business Problem

Traditional banking applications often require users to navigate multiple sections to find account information, transaction details, or answers to banking-related questions.

Users may also need to search through lengthy documentation or FAQs to understand banking services and policies.

This project demonstrates how conversational AI can simplify these interactions through a single interface.

### Proposed Solution

The HDFC Banking AI Assistant combines three approaches:

1. **LLM:** Understands natural-language requests and generates responses.
2. **RAG:** Retrieves relevant information from a curated banking knowledge base.
3. **SQL:** Retrieves structured customer account and transaction information.

By combining these capabilities, the application provides a unified conversational interface for banking-related queries.

---

## 3. Target Users

- Banking customers who need quick access to account and transaction information.
- Users seeking answers to banking FAQs and general banking knowledge.
- Developers exploring Generative AI applications in financial services.
- Evaluators interested in LLM fine-tuning, RAG, and AI application integration.

---

## 4. Key Features

### 4.1 Fine-Tuned LLM

- Base model: Qwen2.5-1.5B-Instruct.
- Fine-tuning approach: LoRA (Low-Rank Adaptation).
- Intent understanding through model inference.
- Integration with the backend through model API endpoints.

### 4.2 Retrieval-Augmented Generation

- Curated banking FAQ knowledge base.
- Document processing and text chunking.
- Semantic embedding generation.
- Vector storage using PostgreSQL and pgvector.
- BM25 keyword-based retrieval.
- Hybrid retrieval using Reciprocal Rank Fusion.
- Context-based answer generation.
- Evidence-aware abstention when sufficient supporting information is unavailable.

### 4.3 SQL-Based Banking Operations

The application supports six predefined database operations:

1. GET_CUSTOMER_ACCOUNTS
2. GET_ACCOUNT_BALANCE
3. GET_TRANSACTION_SUMMARY
4. GET_RECENT_TRANSACTIONS
5. GET_FAILED_TRANSACTIONS
6. GET_PENDING_TRANSACTIONS

These operations allow authenticated users to retrieve information from the synthetic banking database.

### 4.4 Authentication and Session Management

- User login and authentication.
- Argon2 password hashing.
- Session token generation.
- Hashed session-token storage.
- HTTP-only session cookies.
- Authenticated customer-level data access.

### 4.5 Intent-Based Routing

The intent router determines which processing path should handle the user's request.

Supported processing paths:

- SQL
- RAG
- SQL + RAG
- LLM-only
- Out-of-scope handling

### 4.6 Conversational Interface

- Natural-language interaction.
- Frontend-to-backend API communication.
- Banking query processing.
- Structured and context-aware responses.

---

## 5. System Architecture

The application follows a modular architecture in which the FastAPI backend acts as the central controller.

### Architecture Diagram

```text
                    USER
                      |
                      v
             FRONTEND APPLICATION
                      |
                      v
                FASTAPI BACKEND
                      |
                      v
             AUTHENTICATION & SESSION
                      |
                      v
                 CHAT SERVICE
                      |
                      v
                 INTENT ROUTER
                      |
          +-----------+-----------+
          |           |           |
          v           v           v
         SQL         RAG        LLM ONLY
       SERVICE     SERVICE      SERVICE
          |           |           |
          v           v           v
      PostgreSQL   Hybrid       Qwen Model
      Banking DB   Retrieval    Inference API
          |           |           |
          v           v           v
      SQL RESULT   CONTEXT     GENERATED ANSWER
          |           |           |
          +-----------+-----------+
                      |
                      v
              RESPONSE TO USER
```

### Architecture Components

| Component | Responsibility |
|---|---|
| Frontend | User interaction and displaying responses |
| FastAPI | API endpoints and request handling |
| Authentication | Login and session validation |
| Chat Service | Central request orchestration |
| Intent Router | Determines the request processing path |
| SQL Service | Retrieves customer-specific banking data |
| RAG Service | Retrieves relevant banking knowledge |
| LLM Service | Communicates with the Qwen model API |
| PostgreSQL | Stores structured banking data and knowledge |
| pgvector | Stores and searches document embeddings |

---

## 6. Technology Stack

| Category | Technology |
|---|---|
| Programming Language | Python |
| Backend Framework | FastAPI |
| API Server | Uvicorn |
| Database | PostgreSQL |
| Vector Search | pgvector |
| Database Access | SQLAlchemy, Psycopg |
| Base LLM | Qwen2.5-1.5B-Instruct |
| Fine-Tuning | LoRA, PEFT |
| Model Framework | Hugging Face Transformers |
| Embedding Model | BAAI/bge-small-en-v1.5 |
| Keyword Retrieval | BM25 |
| Hybrid Retrieval | Reciprocal Rank Fusion |
| Authentication | Argon2 |
| Frontend | HTML, CSS, JavaScript |
| Model Development | Google Colab |

---

## 7. Fine-Tuned Model

The project uses the Qwen2.5-1.5B-Instruct model with a LoRA adapter.

### Model Configuration

| Parameter | Value |
|---|---|
| Base Model | Qwen2.5-1.5B-Instruct |
| Model Source | Hugging Face |
| Fine-Tuning Method | LoRA |
| Task Type | CAUSAL_LM |
| LoRA Rank | 16 |
| LoRA Alpha | 32 |
| LoRA Dropout | 0.05 |
| Target Modules | q_proj, k_proj, v_proj, o_proj, gate_proj, up_proj, down_proj |

### Fine-Tuning Dataset

The prepared dataset contains 7,126 records.

| Dataset Split | Records |
|---|---:|
| Training | 5,470 |
| Validation | 776 |
| Testing | 880 |
| **Total** | **7,126** |

The dataset contains three task categories:

- ROUTER
- ROUTER_FOLLOWUP
- ANSWER

### Adapter Storage

The fine-tuned adapter is maintained separately from the GitHub source repository because model weights are large.

Local adapter directory:

`backend/models/hdfc_qlora_final/`

The model weights are excluded from GitHub through `.gitignore`.

Refer to `backend/models/README.md` for adapter configuration and loading instructions.

**Note:** The adapter configuration documents LoRA. QLoRA and specific training hyperparameters are not claimed here.

---

## 8. Retrieval-Augmented Generation (RAG)

The RAG pipeline retrieves relevant information from the curated banking knowledge base before generating an answer.

### RAG Workflow

```text
       BANKING FAQ DATASET
                |
                v
       DOCUMENT PROCESSING
                |
                v
          TEXT CHUNKING
                |
                v
       EMBEDDING GENERATION
                |
                v
       POSTGRESQL + PGVECTOR
                |
                v
           USER QUERY
                |
          +-----+------+
          |            |
          v            v
        BM25       VECTOR SEARCH
          |            |
          +-----+------+
                |
                v
       RECIPROCAL RANK FUSION
                |
                v
        RETRIEVED CONTEXT
                |
                v
          LLM GENERATION
                |
                v
         RESPONSE TO USER
```

### Knowledge Base Details

| Parameter | Value |
|---|---|
| Source Workbook | HDFC_Banking_LLM_Final_Curated_Dataset_v1.xlsx |
| Approved Documents | 2,003 |
| Excluded Documents | 1,235 |
| Generated Chunks | 2,074 |
| Embedding Model | BAAI/bge-small-en-v1.5 |
| Embedding Dimension | 384 |
| Vector Storage | PostgreSQL + pgvector |
| Retrieval Methods | BM25 + Vector Search |
| Fusion Method | Reciprocal Rank Fusion |
| Default Top-K | 5 |

### Hybrid Retrieval

The system combines keyword-based and semantic retrieval.

BM25 retrieves documents based on keyword relevance, while vector search identifies semantically similar content.

The results are combined using Reciprocal Rank Fusion (RRF).

The fusion formula is:

`RRF Score(d) = Σ 1 / (60 + rank(d))`

Both retrieval methods use a weight of 1.0, with the RRF constant set to 60.

### RAG Database Tables

| Table | Purpose |
|---|---|
| knowledge.documents | Stores document information |
| knowledge.document_chunks | Stores processed document chunks and embeddings |
| knowledge.embedding_runs | Tracks embedding generation runs |

The retrieval service returns relevant context, source identifiers, verification information, and metadata. It also supports abstention when sufficient evidence is unavailable.

---

## 9. SQL-Based Banking Data Retrieval

The SQL service handles structured banking queries.

### Supported Operations

| Operation | Description |
|---|---|
| GET_CUSTOMER_ACCOUNTS | Retrieves customer accounts |
| GET_ACCOUNT_BALANCE | Retrieves account balance |
| GET_TRANSACTION_SUMMARY | Provides transaction summaries |
| GET_RECENT_TRANSACTIONS | Retrieves recent transactions |
| GET_FAILED_TRANSACTIONS | Retrieves failed transactions |
| GET_PENDING_TRANSACTIONS | Retrieves pending transactions |

### Customer Data Security

Customer identity is derived from the authenticated session rather than being trusted from frontend input.

The SQL service uses predefined allowlisted operations to restrict the queries that can be executed.

This approach helps prevent unauthorized access to another customer's banking information.

---

## 10. Authentication and Session Management

The authentication module manages login and authenticated access.

### Authentication Workflow

```text
        USER LOGIN
             |
             v
     VERIFY CREDENTIALS
             |
             v
      ARGON2 PASSWORD
         VERIFICATION
             |
       +-----+-----+
       |           |
       v           v
     INVALID     VALID
       |           |
       v           v
    REJECT       CREATE
     LOGIN       SESSION
                   |
                   v
            GENERATE TOKEN
                   |
                   v
          STORE TOKEN HASH
                   |
                   v
          SET HTTP-ONLY COOKIE
                   |
                   v
          AUTHENTICATED ACCESS
```

### Security Implementation

- Passwords are hashed using Argon2.
- Session tokens are generated during successful login.
- Token hashes are stored in the database.
- Original session tokens are returned through HTTP-only cookies.
- Authenticated sessions are used to scope customer-specific operations.

For production deployment, HTTPS and secure cookie settings must be enabled.

---

## 11. Intent-Based Request Routing

The intent router determines which service should process a user's request.

### Processing Paths

| User Request Type | Processing Path |
|---|---|
| Account balance | SQL |
| Recent transactions | SQL |
| Banking FAQ | RAG |
| Request requiring both account data and banking knowledge | SQL + RAG |
| General greeting | LLM-only |
| Unsupported request | Out-of-scope handling |

### Request Processing

1. User submits a natural-language query.
2. FastAPI validates the authenticated session.
3. Chat Service forwards the request to the intent router.
4. The intent router predicts the appropriate processing path.
5. The corresponding service executes the request.
6. The result is formatted and returned to the frontend.

---

## 12. API Endpoints

The main API endpoints include:

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/auth/login` | Authenticates a user |
| POST | `/api/chat` | Processes conversational banking queries |

The backend exposes API documentation through FastAPI Swagger UI.

Local Swagger URL:

`http://127.0.0.1:8000/docs`

Refer to the running application for the actual endpoint schemas and additional routes.

---

## 13. Project Directory Structure

```text
HDFC-BANKING-AI/
│
├── backend/
│   │
│   ├── app/
│   │   ├── main.py
│   │   ├── services/
│   │   └── ...
│   │
│   ├── models/
│   │   ├── README.md
│   │   └── hdfc_qlora_final/
│   │       └── ...              # Excluded from GitHub
│   │
│   ├── rag/
│   │   ├── retrieval/
│   │   └── ...
│   │
│   ├── tests/
│   │
│   ├── database/
│   │
│   ├── requirements.txt
│   ├── .env.example
│   └── README.md
│
├── frontend/
│   └── ...
│
├── database/
│   └── ...
│
├── shared/
│   └── ...
│
├── .gitignore
└── README.md
```

*Note: This is a high-level representation. Refer to the repository for the complete implementation and actual directory structure.*

---

## 14. Installation and Local Setup

### Prerequisites

- Python 3.10 or later.
- PostgreSQL.
- pgvector extension.
- Git.
- Node.js, if required by the frontend.
- Access to the configured Qwen model inference API.

### Step 1: Clone the Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd HDFC-BANKING-AI
```

### Step 2: Navigate to Backend

```bash
cd backend
```

### Step 3: Create a Virtual Environment

```bash
python -m venv .venv
```

Activate on Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

### Step 4: Install Dependencies

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### Step 5: Configure Environment Variables

Create a `.env` file using `.env.example` as a reference.

Configure the required environment variables for:

- PostgreSQL database connection.
- Model inference API URL.
- Model API authentication token.
- Application settings.

Never commit `.env` or API credentials to GitHub.

### Step 6: Configure PostgreSQL

1. Create the required PostgreSQL database.
2. Enable the pgvector extension.
3. Execute the database initialization scripts.
4. Configure the database connection in `.env`.
5. Verify database connectivity.

### Step 7: Configure the Model

The application uses the fine-tuned Qwen adapter through the configured model inference service.

Ensure that:

- The Qwen base model is accessible.
- The LoRA adapter is available to the inference service.
- The model API is running.
- The backend has the correct model API URL and credentials.

Refer to `backend/models/README.md` for the adapter loading example.

### Step 8: Start the Backend

From the `backend` directory:

```bash
uvicorn app.main:app --reload
```

Verify the backend using:

`http://127.0.0.1:8000/docs`

### Step 9: Start the Frontend

Follow the frontend-specific setup instructions and start the frontend application.

Configure the frontend API base URL to point to the running FastAPI backend.

**Note:** The exact frontend startup command depends on the frontend files included in the repository.

---

## 15. Environment Configuration

The `.env.example` file provides a template for the required environment variables.

Typical configuration categories include:

| Configuration | Purpose |
|---|---|
| Database URL | PostgreSQL connection |
| Model API URL | Qwen inference endpoint |
| Model API Token | Model API authentication |
| Application Settings | Backend configuration |

Use placeholder values in `.env.example`.

Never publish actual database passwords, API tokens, or other credentials.

---

## 16. Testing and Evaluation

The project includes unit tests and service-level checks for backend and RAG components.

### Testing Areas

- Authentication service.
- Session management.
- Intent routing.
- SQL service.
- LLM service.
- RAG service.
- Chat orchestration.
- Database and vector retrieval checks.

The RAG implementation has 31 reported passing tests, including pgvector smoke tests.

### RAG Retrieval Evaluation

The initial retrieval evaluation used 29 answerable questions.

| Metric | BM25 | Vector | Hybrid RRF |
|---|---:|---:|---:|
| Recall@1 | 51.7% | 37.9% | 44.8% |
| Recall@3 | 55.2% | 55.2% | 51.7% |
| Recall@5 | 55.2% | 62.1% | 55.2% |
| MRR | 0.535 | 0.486 | 0.503 |

### Absent-Answer Evaluation

The initial evaluation reported that 91.7% of absent-answer questions were correctly refused.

### Evaluation Limitations

- The answerable-question evaluation set contains only 29 questions.
- Questions require further human review.
- Retrieval performance requires evaluation on a larger dataset.
- The initial evaluation results are experimental and should not be interpreted as production performance guarantees.

---

## 17. Conversation Handling and Guardrails

### Session Handling

The application uses authenticated sessions to identify users and scope customer-specific SQL operations.

Persistent conversational memory across independent sessions is not claimed as an implemented feature.

### Guardrails

- Customer-specific SQL access is scoped using the authenticated session.
- SQL execution is restricted to predefined operations.
- Synthetic data is used for customer and transaction demonstrations.
- RAG responses use retrieved knowledge as supporting context.
- Insufficient retrieval evidence should trigger the configured abstention or fallback behavior.
- Unsupported requests are handled through out-of-scope routing.

---

## 18. Security Considerations

The prototype includes several basic security measures:

- Argon2 password hashing.
- Hashed session-token storage.
- HTTP-only session cookies.
- Authenticated customer-level access.
- Allowlisted SQL operations.
- Environment-based secret configuration.

### Production Security Requirements

Before production use, additional work is required, including:

- HTTPS and secure cookie configuration.
- Rate limiting.
- Comprehensive security testing.
- Monitoring and audit logging.
- Improved error handling.
- API authentication and authorization hardening.
- Protection against prompt injection and malicious input.
- Production-grade secret management.

---

## 19. Current Limitations

This project is an educational prototype and has not been designed or validated for production banking operations.

Known limitations include:

1. Uses synthetic customer and transaction data.
2. Not integrated with live HDFC banking infrastructure.
3. Model inference depends on the availability of the configured API endpoint.
4. The current model API may require an active Google Colab session.
5. RAG evaluation is based on a limited test dataset.
6. Some database and end-to-end integration checks require further verification.
7. Production-grade monitoring, high availability, autoscaling, and disaster recovery are not implemented.
8. Comprehensive production security testing has not been completed.

---

## 20. Future Enhancements

- Deploy the application using production-ready infrastructure.
- Improve intent classification accuracy.
- Expand the curated banking knowledge base.
- Improve hybrid retrieval relevance.
- Expand the evaluation dataset.
- Introduce automated model evaluation.
- Add monitoring, logging, and rate limiting.
- Improve failure handling and API reliability.
- Strengthen authentication and authorization.
- Add production-grade security controls.

## 21. Project Demonstration

**GitHub Repository:** Add your final GitHub repository URL.

**Video Demonstration:** https://drive.google.com/file/d/10pAoXIWRaq8IRXA9aI05vL4GopPdWMtQ/view?usp=drive_link

**Live Application:** Not deployed yet due to lack of resources.

---

## 22. Disclaimer

This project is developed for educational and demonstration purposes as part of an Applied Generative AI project.

It uses synthetic banking customer and transaction data. It does not access real HDFC Bank customer information, accounts, or banking infrastructure.

The application does not perform actual financial transactions, fund transfers, or account modifications.

It is not an official HDFC Bank product and must not be used for real banking operations.

---

## 23. Acknowledgements

- Hugging Face for open-source model and machine learning tools.
- Qwen team for the Qwen2.5 model.
- BAAI for the BGE embedding model.
- FastAPI and PostgreSQL communities.
- AlmaBetter for the Applied Generative AI learning program.

---

**Developed as an Applied Generative AI banking assistant prototype.**
