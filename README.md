<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Source-Code Analysis Chatbot – Project README</title>
  <style>
    :root{
      --bg:#0f172a;--card:#111827;--ink:#e5e7eb;--muted:#9ca3af;--accent:#60a5fa;--ok:#34d399;--warn:#fbbf24
    }
    *{box-sizing:border-box}body{margin:0;font:16px/1.6 system-ui,-apple-system,Segoe UI,Roboto,Ubuntu,"Helvetica Neue",Arial;color:var(--ink);background:linear-gradient(180deg,#0b1220,#0f172a)}
    .wrap{max-width:960px;margin:auto;padding:40px 20px}
    .card{background:rgba(17,24,39,.7);backdrop-filter: blur(6px);border:1px solid rgba(255,255,255,.06);border-radius:18px;padding:28px;margin:18px 0}
    h1,h2,h3{line-height:1.25;margin:0 0 .4em}
    h1{font-size:2rem}h2{font-size:1.35rem;margin-top:1.2em}
    code,kbd,pre{font-family:ui-monospace,SFMono-Regular,Menlo,Monaco,Consolas,"Liberation Mono","Courier New",monospace}
    pre{background:#0b1020;border:1px solid rgba(255,255,255,.06);border-radius:14px;padding:14px;overflow:auto}
    .badges span{display:inline-block;padding:.32em .6em;border-radius:999px;border:1px solid rgba(255,255,255,.14);margin:.2em .3em;color:#d1d5db}
    .pill{display:inline-block;background:rgba(96,165,250,.15);border:1px solid rgba(96,165,250,.35);color:#dbeafe;padding:.2em .6em;border-radius:999px;margin-left:.5ch}
    a{color:#93c5fd;text-decoration:none}a:hover{text-decoration:underline}
    ul{margin:.2em 0 .6em 1.2em}
    .grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(260px,1fr));gap:14px}
    .small{color:var(--muted);font-size:.95rem}
    .table{width:100%;border-collapse:separate;border-spacing:0 6px}
    .table td,.table th{padding:.55rem .7rem;text-align:left}
    .row{background:rgba(255,255,255,.03);border:1px solid rgba(255,255,255,.06);border-radius:10px}
    .kbd{border:1px solid rgba(255,255,255,.15);border-bottom-width:2px;border-radius:8px;padding:.05rem .4rem;background:#0b1020}
    .note{border-left:4px solid var(--accent);padding:.6em .9em;background:rgba(96,165,250,.08);border-radius:10px}
  </style>
</head>
<body>
  <main class="wrap">
    <header class="card">
      <h1>Source‑Code Analysis Chatbot</h1>
      <p class="small">Conversational code understanding over any Python GitHub repository using <strong>LangChain</strong>, <strong>OpenAI embeddings</strong>, and <strong>ChromaDB</strong>, with a <strong>Flask</strong> backend and simple <strong>HTML/CSS/JS</strong> frontend.</p>
      <div class="badges">
        <span>Python 3.10+</span>
        <span>Flask</span>
        <span>LangChain</span>
        <span>ChromaDB</span>
        <span>OpenAI API</span>
        <span>GitPython</span>
        <span>HTML · CSS · JS</span>
      </div>
    </header>

    <section class="card">
      <h2>Overview</h2>
      <ul>
        <li>Clone any public GitHub repo containing <code>.py</code> files.</li>
        <li>Parse and split code into semantic chunks (500 chars, 20 overlap).</li>
        <li>Embed chunks with <code>OpenAIEmbeddings</code> and persist to <strong>ChromaDB</strong>.</li>
        <li>Ask natural‑language questions; retrieval‑augmented answers are produced by <code>ChatOpenAI</code>.</li>
        <li>Conversation memory summarizes prior turns for context‑aware replies.</li>
      </ul>
    </section>

    <section class="card">
      <h2>Tech Stack</h2>
      <div class="grid">
        <div>
          <h3>Backend</h3>
          <ul>
            <li>Flask (API & routing)</li>
            <li>GitPython (repo cloning)</li>
            <li>LangChain: loaders, splitters, retriever, chains</li>
            <li>ChromaDB (vector store)</li>
            <li>OpenAI (Chat & Embeddings)</li>
            <li>dotenv (config)</li>
          </ul>
        </div>
        <div>
          <h3>Frontend</h3>
          <ul>
            <li>HTML templates (<code>templates/index.html</code>)</li>
            <li>CSS for styling</li>
            <li>JavaScript (fetch‑based chat & repo submit)</li>
          </ul>
        </div>
      </div>
    </section>

    <section class="card">
      <h2>Architecture</h2>
      <p class="small">End‑to‑end flow from user to LLM‑augmented retrieval.</p>
      <pre><code>User (Frontend) → Flask API
  ├─ POST <kbd class="kbd">/chatbot</kbd>  (clone repo URL)
  │    └─ GitPython → repo/ → <kbd class="kbd">store_index.py</kbd>
  │          └─ Load .py → Split → OpenAIEmbeddings → ChromaDB (db/)
  └─ POST <kbd class="kbd">/get</kbd> (ask question)
       └─ ConversationalRetrievalChain (Chroma retriever + ChatOpenAI + Memory)
            └─ Answer → Frontend
</code></pre>
    </section>

    <section class="card">
      <h2>Quick Start</h2>
      <ol>
        <li>Install dependencies:
          <pre><code>pip install -r requirements.txt</code></pre>
        </li>
        <li>Set environment:
          <pre><code>cp .env.example .env
# then edit .env
OPENAI_API_KEY=&lt;your-key&gt;</code></pre>
        </li>
        <li>Run the app:
          <pre><code>python app.py  # serves at http://localhost:8080</code></pre>
        </li>
      </ol>
      <div class="note small">
        If you submit a new repo URL via <kbd class="kbd">/chatbot</kbd>, the app will clone into <code>repo/</code> and rebuild the index into <code>db/</code> by running <code>store_index.py</code>.
      </div>
    </section>

    <section class="card">
      <h2>API Endpoints</h2>
      <table class="table">
        <thead>
          <tr class="row"><th>Method & Path</th><th>Description</th><th>Body</th></tr>
        </thead>
        <tbody>
          <tr class="row"><td><code>GET /</code></td><td>Serves chat UI.</td><td>—</td></tr>
          <tr class="row"><td><code>POST /chatbot</code></td><td>Clone & index a GitHub repo.</td><td><code>question=&lt;repo_url&gt;</code></td></tr>
          <tr class="row"><td><code>POST /get</code></td><td>Ask a question about the indexed repo.</td><td><code>msg=&lt;your question&gt;</code></td></tr>
        </tbody>
      </table>
    </section>

    <section class="card">
      <h2>Project Structure</h2>
      <pre><code>.
├── app.py
├── store_index.py
├── src/
│   └── helper.py           # repo_ingestion, load_repo, text_splitter, load_embedding
├── templates/
│   └── index.html          # chat UI
├── static/
│   ├── style.css
│   └── script.js
├── db/                     # ChromaDB persistent store
├── repo/                   # cloned repository
└── .env                    # OPENAI_API_KEY
</code></pre>
    </section>

    <section class="card">
      <h2>Security & Notes</h2>
      <ul>
        <li>Only clone trusted repositories; treat cloned code as untrusted.</li>
        <li>Your OpenAI usage incurs cost; monitor tokens and rate limits.</li>
        <li>Clear the cloned repo with <code>msg=clear</code> to remove <code>repo/</code>.</li>
      </ul>
    </section>

    <section class="card">
      <h2>License</h2>
      <p class="small">MIT (or your preferred license). Remember to include third‑party licenses if you redistribute.</p>
    </section>

    <footer class="small" style="opacity:.7;padding:8px 2px">
      Built with ❤️ using Flask · LangChain · ChromaDB · OpenAI.
    </footer>
  </main>
</body>
</html>
