# 👑 AI For DevOps

> *"The era of manually writing YAML files, bash scripts, and clicking through cloud consoles is fading. The future of Cloud and DevOps belongs to the Architects who build the autonomous systems that write the scripts for them. This repository is not a basic tutorial—it is a forge. It is meticulously designed to break you out of 'tutorial hell' and elevate you from a standard Cloud Operator into an Elite AI Platform Engineer. Do not rush. Master the fundamentals, build the tools, stay disciplined, and welcome to the Top 1%."*

---

## 🗂️ Repository Architecture & Learning Path

### 📁 `01_Python_for_DevOps_and_Cloud`
The native language of Artificial Intelligence and advanced cloud automation. 
*   📂 **`01_Basic/`**
    *   📄 `01_Syntax_and_Data_Structures` *(Lists, Dicts, Sets, Tuples)*
    *   📄 `02_File_Handling_and_Parsing` *(Parsing JSON, YAML, CSV)*
    *   📄 `03_Error_Handling` *(try/except/finally for resilient scripts)*
    *   📄 `04_Environment_Management` *(venv, pip, requirements isolation)*
*   📂 **`02_Intermediate/`**
    *   📄 `01_API_Integration` *(Deep dive into the `requests` library & webhooks)*
    *   📄 `02_OS_and_System_Execution` *(Subprocess execution of kubectl/terraform)*
    *   📄 `03_Concurrency` *(Using `asyncio` for multi-account AWS queries)*
    *   📄 `04_Object_Oriented_Programming` *(Classes for modular script design)*
*   📂 **`03_Top_1_Percent/`**
    *   📄 `01_AWS_Boto3_Mastery` *(Programmatic AWS infrastructure management)*
    *   📄 `02_CLI_Development` *(Building internal tools using Typer/Click)*
    *   📄 `03_Infrastructure_Testing` *(Unit testing cloud code with Pytest & Moto)*
    *   📄 `04_Kubernetes_Automation` *(Writing Python K8s Operators with Kopf)*

---

### 📁 `02_LangChain_and_LlamaIndex`
The core frameworks for building AI Agents and RAG (Retrieval-Augmented Generation) applications.
*   📂 **`01_Basic/`**
    *   📄 `01_LLM_API_Integration` *(Connecting OpenAI/Anthropic/Ollama)*
    *   📄 `02_Prompt_Engineering` *(Dynamic variable injection & templating)*
    *   📄 `03_Basic_Chains` *(Linking prompts to LLM execution)*
*   📂 **`02_Intermediate/`**
    *   📄 `01_Document_Loaders` *(Ingesting Confluence, Jira, and GitHub data)*
    *   📄 `02_Text_Splitters` *(Chunking strategies for massive repositories)*
    *   📄 `03_Embeddings_Generation` *(Converting text into vector math)*
    *   📄 `04_Basic_RAG` *(Querying private company data securely)*
*   📂 **`03_Top_1_Percent/`**
    *   📄 `01_Custom_Tools_and_Agents` *(Giving AI 'hands' to run terminal commands)*
    *   📄 `02_Structured_Output` *(Enforcing strict JSON outputs via Pydantic)*
    *   📄 `03_Agentic_Workflows` *(Multi-agent loops using LangGraph)*
    *   📄 `04_AI_Observability` *(Monitoring token usage & latency via LangSmith)*

---

### 📁 `03_Building_AI_DevOps_Tools`
Integrating LLM intelligence directly into infrastructure deployment and CI/CD.
*   📂 **`01_Basic/`**
    *   📄 `01_Authentication_Security` *(Managing API keys via Secrets Manager)*
    *   📄 `02_Stateless_REST_Calls` *(Basic chat completion endpoint interactions)*
    *   📄 `03_Rate_Limiting_Retries` *(Exponential backoff for API limits)*
*   📂 **`02_Intermediate/`**
    *   📄 `01_System_Prompts_Context` *(Crafting rigid persona constraints)*
    *   📄 `02_Streaming_Responses` *(Word-by-word CLI output streams)*
    *   📄 `03_File_IO_Integration` *(Auto-saving generated code to disk)*
*   📂 **`03_Top_1_Percent/`**
    *   📄 `01_Function_Calling` *(Triggering actual infra changes from LLMs)*
    *   📄 `02_CICD_Integration` *(Building an automated AI PR Code Reviewer)*
    *   📄 `03_Sandboxing_Security` *(Testing AI-generated scripts in isolated containers)*

---

### 📁 `04_Vector_Databases`
The storage engines required to give AI long-term memory and contextual awareness.
*   📂 **`01_Basic/`**
    *   📄 `01_Vector_Math_Fundamentals` *(Cosine similarity & Euclidean distance)*
    *   📄 `02_Local_Deployment` *(Running pgvector via Docker Compose)*
    *   📄 `03_Basic_CRUD_Operations` *(Inserting and querying vector embeddings)*
*   📂 **`02_Intermediate/`**
    *   📄 `01_AWS_RDS_Integration` *(Deploying pgvector in production PostgreSQL)*
    *   📄 `02_Vector_Indexing` *(HNSW vs IVFFlat for query speed)*
    *   📄 `03_ORM_Integration` *(Using SQLAlchemy for similarity searches)*
*   📂 **`03_Top_1_Percent/`**
    *   📄 `01_High_Availability_Deployments` *(Deploying Milvus/Qdrant on EKS)*
    *   📄 `02_Hybrid_Search` *(Combining Keyword BM25 with Vector Similarity)*
    *   📄 `03_Vector_Data_Lifecycle` *(Zero-downtime snapshotting and DB backups)*

---

### 📁 `05_MLOps_Foundations`
The CI/CD pipelines, hardware provisioning, and lifecycle management for AI Models.
*   📂 **`01_Basic/`**
    *   📄 `01_The_AI_Pipeline` *(Data Prep vs Training vs Fine-Tuning vs Inference)*
    *   📄 `02_Compute_Architecture` *(Why GPUs and CUDA matter)*
    *   📄 `03_Containerizing_AI` *(Writing optimal Dockerfiles for massive ML payloads)*
*   📂 **`02_Intermediate/`**
    *   📄 `01_Model_Serving` *(Wrapping ML models into REST APIs using FastAPI)*
    *   📄 `02_AWS_SageMaker_Basics` *(Provisioning and deploying via SDK)*
    *   📄 `03_Model_Storage` *(Managing 50GB+ model weights in S3)*
*   📂 **`03_Top_1_Percent/`**
    *   📄 `01_GPU_Provisioning_EKS` *(Dynamic scaling of p4d/g5 nodes with Karpenter)*
    *   📄 `02_NVIDIA_K8s_Integration` *(Device plugins for hardware-level pod access)*
    *   📄 `03_KubeFlow_Orchestration` *(Distributed ML training on Kubernetes)*
    *   📄 `04_Model_Observability` *(Tracking data drift and decay with MLflow)*