# Under-The-Hood
Built as a project to show how production vector databases like Pinecone, Weaviate, and Chroma actually work under the hood
# Under The Hood: A From-Scratch Vector Database

**Under The Hood** is a fully functional, open-source vector database built completely from scratch in C++. Running entirely on a local Windows machine and featuring an interactive web-based UI. It deconstructs and demonstrates the underlying computational mechanics, data structures, and algorithmic trade-offs that power production-grade vector databases like Pinecone, Milvus, and Chroma.

---

## 🚀 Overview 

Modern LLM workflows rely heavily on semantic retrieval, yet the databases powering them often operate as "black boxes." **Under The Hood** strips away abstract wrappers to show exactly how high-dimensional vectors are indexed, partitioned, queried, and mapped visually in real-time. 

By comparing production-grade graph topologies against classic spatial trees and brute-force baselines, this project acts as a living sandbox for understanding indexing performance, distance metrics, and the mathematical "curse of dimensionality."

---

## ✨ Core Features

### 1. Three Pluggable Search Algorithms
* **HNSW (Hierarchical Navigable Small World):** A production-grade, approximate nearest neighbor (ANN) graph algorithm. By constructing a multi-layered graph of skip-list style connections, it achieves lightning-fast retrieval in $O(\log N)$ time.
* **KD-Tree (K-Dimensional Tree):** An exact binary space partitioning tree optimized for lower dimensions. It demonstrates how traditional geometric indexers perform under spatial constraints.
* **Brute Force (Linear Scan):** An exact baseline search that calculates distances sequentially across every vector in the dataset ($O(N)$ time). Used as the ground-truth benchmark for accuracy.

### 2. Built-in Local RAG Pipeline
The project implements a complete, network-isolated Retrieval-Augmented Generation (RAG) workflow powered by **Ollama**:
1.  **Embedding Generation:** User text and document chunks are converted into 768-dimensional embeddings using the `nomic-embed-text` model.
2.  **Vector Indexing:** Document embeddings are structuralized directly into the C++ HNSW graph index.
3.  **Semantic Retrieval & Synthesis:** Incoming queries are vectorized, the closest relevant document context is retrieved via semantic search, and a structured prompt is dispatched to `llama3.2` to generate clean, context-aware answers.

### 3. Interactive Web Interface & Developer API
* **Side-by-Side Performance Benchmarking:** A live metric dashboard allowing real-time benchmarking of search speed (latency) and accuracy (recall) across three foundational distance metrics: **Cosine**, **Euclidean**, and **Manhattan** distances.
* **Visual 2D Clustering:** Includes a Principal Component Analysis (PCA) engine that projects 768-dimensional vectors onto a 2D canvas, allowing users to visually watch semantic categories form distinct clusters.
* **REST API:** A clean developer interface exposing explicit endpoints for structural data insertion, item deletion, vector queries, and system performance diagnostics.

---

## 📊 Technical Performance Breakdown

The project explicitly showcases how different indexes behave when subjected to high-dimensional datasets (such as 768-dimensional embeddings):

| Algorithm | Query Complexity | Efficiency in High Dimensions (768D) | Operational Caveat |
| :--- | :--- | :--- | :--- |
| **HNSW** | $O(\log N)$ | **Excellent**. Uses its multi-layered graph topology to quickly navigate high-dimensional spaces without structural degradation. | Approximate results; requires graph construction overhead. |
| **KD-Tree** | $O(\log N)$ | **Poor**. Suffers heavily from the *"Curse of Dimensionality"*. Geometric pruning fails in high dimensions, causing it to revert to an $O(N)$ brute-force scan. | Ideal for low-dimensional spatial indexing ($D \le 10$). |
| **Brute Force**| $O(N \cdot d)$ | **Baseline**. Computationally heavy, but provides $100\%$ exact precision benchmarks. | Scale limitations; computation time grows linearly with dataset size ($N$). |


---

