About The Project

VectorMind RAG is an intelligent question-answering system built using Retrieval
Augmented Generation (RAG) — a powerful AI technique that allows a Large
Language Model to answer questions based on your own documents rather than just
its pre-trained knowledge.

Instead of asking Gemini a question directly, this system first searches a vector database for the most relevant document chunks related to your question, then passes those chunks as context to the LLM. This makes the answers far more accurate, grounded, and traceable back to real source documents.
This project is built using LangChain for orchestration, ChromaDB as the persistent vector store, Google Gemini for both embeddings and text generation, and Python as the core language.

What Problem Does This Solve?
Large Language Models like Gemini or GPT are powerful but they have a major limitation — they only know what they were trained on. If you want them to answer questions about your own documents, your company's data, or any private information, you cannot just ask the model directly.
RAG solves this by giving the LLM access to your documents at query time. The system converts your documents into vector embeddings and stores them in ChromaDB. When a user asks a question, the system finds the most similar document chunks using vector similarity search and sends them to the LLM as context. The LLM then generates an answer strictly based on those retrieved documents.
This is the exact architecture used in real enterprise AI products like customer support bots, internal knowledge assistants, and document search tools.

How It Works — Step by Step
Step 1 — User asks a question
The user provides a natural language query. For example: "Where is Dracula's castle located?"
Step 2 — Query is converted to a vector embedding
The question is passed to Google's Gemini Embedding model (gemini-embedding-001) which converts it into a dense numerical vector that captures the semantic meaning of the question.
Step 3 — Vector similarity search in ChromaDB
The query vector is compared against all document vectors stored in the ChromaDB persistent database. The system retrieves the top 3 most semantically similar document chunks that have a similarity score of at least 0.2. Any document below this threshold is filtered out to avoid irrelevant results.
Step 4 — Retrieved documents are shown with metadata
Each retrieved document chunk is displayed along with its source file name so you know exactly which document the information came from. This is the metadata-aware part of the system.
Step 5 — Prompt is constructed and sent to Gemini
The retrieved document chunks are combined with the original question into a structured prompt. A system message tells the model to act as a helpful assistant. The human message contains the question plus all the retrieved context.
Step 6 — Gemini generates the final answer
Google Gemini 2.5 Flash reads the context documents and generates a grounded answer. If the answer is not found anywhere in the retrieved documents, the model responds with "I'm not sure" instead of hallucinating a wrong answer. This is a critical safety feature in production RAG systems.

Features

Persistent Vector Store using ChromaDB — embeddings are stored on disk so you do not re-embed documents every time you run the script
Metadata tracking on every document chunk — you always know which source file an answer came from
Similarity score threshold filtering — only retrieves documents above a minimum relevance score, reducing noise
Google Gemini 2.5 Flash for generation — fast, accurate, and cost-effective LLM
Google Gemini Embedding 001 for embeddings — state-of-the-art semantic embeddings
LangChain retriever abstraction — clean and modular pipeline that is easy to extend
Secure API key management using dotenv — no hardcoded credentials
Honest fallback response — model says "I'm not sure" when the answer is not in the documents


Tech Stack

Python 3.10+ — Core programming language
LangChain — RAG pipeline and retriever orchestration
langchain-chroma — ChromaDB integration for LangChain
langchain-google-genai — Google Gemini LLM and embedding integration
ChromaDB — Local persistent vector database
Google Gemini 2.5 Flash — Large Language Model for answer generation
Google Gemini Embedding 001 — Text embedding model
python-dotenv — Environment variable management

