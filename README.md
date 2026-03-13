# 🔍 Semantic Search Engine — Powered by Qdrant

<div align="center">

![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Qdrant](https://img.shields.io/badge/Qdrant-Vector_DB-DC244C?style=for-the-badge&logo=qdrant&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Poetry](https://img.shields.io/badge/Poetry-Dependency_Mgmt-60A5FA?style=for-the-badge&logo=poetry&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-brightgreen?style=for-the-badge)

**A full-stack semantic search engine built with Qdrant vector database — find startups by meaning, not just keywords.**

[Live Demo](#) · [Report Bug](../../issues) · [Request Feature](../../issues)

</div>

---

## 📌 Table of Contents

- [About the Project](#-about-the-project)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [System Architecture](#-system-architecture--data-flow)
- [Screenshots](#-screenshots)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation & Setup](#installation--setup)
- [Using a Larger Dataset (Crunchbase)](#-using-a-larger-dataset-with-crunchbase)
- [Project Structure](#-project-structure)
- [How Semantic Search Works](#-how-semantic-search-works)
- [Future Improvements](#-future-improvements)
- [Contributing](#-contributing)
- [License](#-license)
- [Contact](#-contact)

---

## 📖 About the Project

**Qdrant Demo** is a semantic search engine that goes beyond keyword matching — it understands the *meaning* behind your query and returns the most contextually relevant startup results.

Built on top of [Qdrant](https://qdrant.tech/), a high-performance vector similarity search engine, this project demonstrates how to embed natural language queries, store vector representations, and retrieve semantically similar documents at scale — all running locally via Docker.

> 💡 Semantic search finds results based on **intent and context**, not just exact word matches. Ask "AI-powered productivity tools" and it surfaces relevant startups even if they never use those exact words.

---

## ✨ Features

- 🧠 **Semantic vector search** — queries matched by meaning using dense embeddings
- 🚀 **Qdrant vector database** — fast approximate nearest neighbor (ANN) search at scale
- 🐳 **Fully Dockerized** — spin up the entire stack with a single command
- 📦 **Poetry dependency management** — reproducible, conflict-free environments
- 🗂️ **Two dataset options** — lightweight startup JSON or full Crunchbase organizations dataset
- 🌐 **Web frontend** — clean search UI accessible at `localhost:8000`
- ⚡ **Real-time results** — sub-second query response for startup discovery

---

## 🛠 Tech Stack

| Layer              | Technology                        |
|--------------------|-----------------------------------|
| **Vector Database**| Qdrant                            |
| **Backend**        | Python 3.11                       |
| **Dependency Mgmt**| Poetry                            |
| **Containerization**| Docker, Docker Compose           |
| **Embeddings**     | Sentence Transformers / OpenAI    |
| **Dataset**        | Startups JSON / Crunchbase CSV    |
| **Frontend**       | HTML, CSS, JavaScript             |

---

## 🏗 System Architecture & Data Flow

![Semantic Search Engine using Qdrant](https://github.com/user-attachments/assets/8290acb1-51e5-43dd-845b-39cd795ab5be)

```
Raw Data (JSON / CSV)
        │
        ▼
  Text Preprocessing
        │
        ▼
  Embedding Model → Dense Vectors
        │
        ▼
  Qdrant Collection (Vector Index)
        │
        ▼
  User Query → Embed Query Vector
        │
        ▼
  ANN Search (Cosine Similarity)
        │
        ▼
  Top-K Relevant Startups Returned
        │
        ▼
  Results Rendered in Web UI
```

---

## 🖼 Screenshots

### Frontend — Starting Page
![Frontend starting page](https://github.com/user-attachments/assets/4a2ac68d-0345-453b-a973-f95923d47a37)

### Results After Search
![Search results](https://github.com/user-attachments/assets/da391084-0b72-49d8-bbbb-543da0f5cece)

---

## 🚀 Getting Started

### Prerequisites

Ensure the following are installed before proceeding:

| Tool | Version | Link |
|------|---------|------|
| Python | 3.11 | [Download](https://www.python.org/downloads/) |
| Docker & Docker Compose | Latest | [Download](https://www.docker.com/get-started/) |
| pip | Latest | Bundled with Python |
| wget | Any | [Install guide](https://www.gnu.org/software/wget/) |

---

### Installation & Setup

Follow these steps in order:

**Step 1 — Create and activate a virtual environment:**

```bash
python -m venv .venv
source .venv/bin/activate          # macOS/Linux
.venv\Scripts\activate             # Windows
```

**Step 2 — Install dependencies with Poetry:**

```bash
pip install poetry
poetry install
```

**Step 3 — Download the startup dataset:**

```bash
wget https://storage.googleapis.com/generall-shared-data/startups_demo.json -P data/
```

**Step 4 — Start the Qdrant service via Docker:**

```bash
docker-compose -f docker-compose-local.yaml up
```

> ⏳ Wait for the Qdrant service to fully start before proceeding to the next step.

**Step 5 — Index the data into Qdrant:**

```bash
python -m qdrant_demo.init_collection_startups
```

**Step 6 — Open the application:**

```
http://localhost:8000/
```

You're ready to search! 🎉

---

## 📊 Using a Larger Dataset with Crunchbase

For a richer search experience, you can index the full **Crunchbase organizations dataset**.

> ⚠️ Requires a free Crunchbase account and API key. Register at [crunchbase.com](https://www.crunchbase.com/).

**Step 1 — Download the Crunchbase data:**

```bash
wget 'https://api.crunchbase.com/odm/v4/odm.tar.gz?user_key=<YOUR-CRUNCHBASE-API-KEY>' -O odm.tar.gz
```

**Step 2 — Extract and move the organizations file:**

```bash
tar -xvf odm.tar.gz
mv odm/organizations.csv ./data
```

**Step 3 — Index the Crunchbase data into Qdrant:**

```bash
python -m qdrant_demo.init_collection_crunchbase
```

> The Crunchbase dataset contains significantly more companies and produces richer, more diverse search results.

---

## 📁 Project Structure

```
qdrant_demo/
│
├── docker-compose-local.yaml        # Docker Compose config for local Qdrant instance
├── pyproject.toml                   # Poetry project config & dependencies
├── poetry.lock                      # Locked dependency versions
│
├── qdrant_demo/                     # Main application package
│   ├── __init__.py
│   ├── init_collection_startups.py  # Index startup JSON dataset into Qdrant
│   ├── init_collection_crunchbase.py# Index Crunchbase CSV into Qdrant
│   ├── search.py                    # Semantic search logic & query embedding
│   └── app.py                       # Web server & API routes
│
├── data/                            # Dataset directory
│   ├── startups_demo.json           # Default startup dataset
│   └── organizations.csv            # (Optional) Crunchbase dataset
│
├── static/                          # Frontend static assets
│   ├── css/
│   └── js/
│
├── templates/                       # HTML templates
│   └── index.html
│
└── README.md
```

---

## 🧠 How Semantic Search Works

Traditional keyword search looks for **exact word matches**. Semantic search works differently:

| Step | What Happens |
|------|-------------|
| 1. **Embed documents** | Each startup description is converted into a dense vector (e.g., 384 or 1536 dimensions) using a sentence embedding model |
| 2. **Store in Qdrant** | Vectors are indexed in a Qdrant collection optimized for ANN (Approximate Nearest Neighbor) search |
| 3. **Embed the query** | Your search query is converted into a vector using the **same** embedding model |
| 4. **Similarity search** | Qdrant finds the top-K vectors closest to your query vector using cosine similarity |
| 5. **Return results** | The matching startup records are returned and displayed in the UI |

This means queries like *"sustainable energy solutions"* will surface relevant startups even if their descriptions use words like *"clean tech"* or *"renewable power"* instead.

---

## 🔮 Future Improvements

- [ ] Add filters — search by industry, country, founding year
- [ ] Hybrid search — combine semantic + keyword (BM25) ranking
- [ ] Pagination and infinite scroll for large result sets
- [ ] REST API with OpenAPI/Swagger documentation
- [ ] Dockerize the full application (frontend + backend + Qdrant)
- [ ] Support for custom embedding models (OpenAI, Cohere, local models)
- [ ] Authentication for multi-user deployments

---

## 🤝 Contributing

Contributions are welcome!

1. **Fork** the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m "Add: your feature description"`
4. Push to your branch: `git push origin feature/your-feature`
5. Open a **Pull Request**

Please follow [PEP 8](https://peps.python.org/pep-0008/) style guidelines and add comments for any non-obvious logic.

---

## 📄 License

Distributed under the **MIT License**. See `LICENSE` for more information.

---

## 📬 Contact

**Your Name** — [ganesh1a0576@gmail.com](mailto:ganesh1a0576@gmail.com)

GitHub: [Ganesh-a0576](https://github.com/Ganesh-a0576)

Project Link: [https://github.com/your-username/qdrant-demo](https://github.com/Ganesh-a0576/Ai-Semantic-Search-Recommendation-Engine)

---

<div align="center">

⭐ Found this useful? Give it a star — it helps others discover the project!

<br/>

Built with ❤️ using Qdrant & Python

</div>
