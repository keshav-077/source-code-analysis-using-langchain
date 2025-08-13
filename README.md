# Source Code Analysis Chatbot

An AI-powered web application that analyzes and explains Python code from GitHub repositories in natural language. Built using **LangChain**, **OpenAI API**, and **ChromaDB**, it enables developers to explore unfamiliar codebases with conversational Q&A.

## 🚀 Features
- **GitHub Repository Ingestion** – Clone and process any public repo.
- **Code Parsing & Chunking** – Extracts Python files and splits code into semantic chunks.
- **Vector Embeddings** – Converts code into embeddings using OpenAI.
- **Semantic Search** – Retrieves the most relevant code snippets from ChromaDB.
- **Conversational Q&A** – Uses an LLM to explain code in plain English with memory.
- **Interactive Frontend** – HTML/CSS/JavaScript chat interface.

## 🛠️ Technologies Used
- **Backend:** Python, Flask, LangChain, GitPython, ChromaDB
- **AI Models:** OpenAI ChatOpenAI, OpenAI Embeddings
- **Frontend:** HTML, CSS, JavaScript
- **Other Tools:** dotenv, RecursiveCharacterTextSplitter

## 📂 Project Structure
```
├── app.py                  # Flask app entry point
├── helper.py               # Utility functions for repo loading & embeddings
├── store-index.py          # Processes and stores code embeddings in ChromaDB
├── templates/              # Frontend HTML files
├── static/                 # CSS & JS files
├── db/                     # Persisted ChromaDB data
└── repo/                   # Cloned repositories
```

## ⚙️ How It Works
1. **Input Repo URL** → Clone the repository.
2. **Parse Python Files** → Extract `.py` files into documents.
3. **Split into Chunks** → Create overlapping code segments.
4. **Embed & Store** → Convert to vectors and store in ChromaDB.
5. **Ask Questions** → Retrieve relevant code and answer with LLM.

## 💻 Setup & Run
```bash
# Clone this project
git clone <your-repo-url>
cd <project-folder>

# Install dependencies
pip install -r requirements.txt

# Set up environment variables
echo "OPENAI_API_KEY=your_api_key_here" > .env

# Start the app
python app.py
```
Open **http://localhost:8080** in your browser.

## 📸 Process Flow


<!-- Example: Hosted image (direct link) -->
<img src="images/ChatGPT Image Aug 13, 2025, 05_47_58 AM.png" alt="Process Flow" width="600">




