<h1>GenAI-Powered Hybrid Log Classification System using Regex, BERT & LLMs</h1>

<p>
This project implements a hybrid log classification system using Regex rules, BERT with logistic regression, and LLMs (Large Language Models) to handle logs ranging from predictable patterns to rare and ambiguous cases. The pipeline adapts dynamically to data availability and complexity.
</p>

<h2>📌 Key Features</h2>
<ul>
  <li>🔬 <strong>Regex-based Classification</strong> for fast, rule-based detection of standard logs.</li>
  <li>🤖 <strong>BERT + Logistic Regression</strong> for handling complex logs with enough labeled data.</li>
  <li>🧠 <strong>LLM (e.g., LLaMA, DeepSeek)</strong> for few-shot inference where traditional training is not viable.</li>
  <li>🧾 <strong>CSV Upload via FastAPI</strong> and output classification results in real time.</li>
  <li>🧠 <strong>Backed by Sentence Transformers, Joblib models, and LLMs via Groq API.</strong></li>
</ul>

<h2>📂 Folder Structure</h2>
<pre><code>
Log-Classification-System/
│
├── models/                  # Contains trained BERT + Logistic Regression models
│   └── models.joblib
│
├── resources/               # Input/output log files
│   ├── test.csv
│   └── output.csv
│
├── training/                # Scripts and dataset for BERT model training
│   ├── dataset/
│   │   └── synthetic_logs.csv
│   └── training.ipynb
│
├── classify.py              # Main orchestration logic for 3-stage classification
├── processor_regex.py       # Regex-based classification processor
├── processor_bert.py        # BERT + Logistic Regression classification processor
├── processor_llm.py         # LLM (Groq API) based fallback classifier
├── server.py                # FastAPI app to serve classification endpoint
├── main.py                  # Testing / Demo script
├── requirements.txt         # Dependencies
└── .env                     # API Keys and Configs (excluded from Git)
</code></pre>

<h2>🧠 Classification Strategy</h2>
<p>
Logs are classified in a 3-stage pipeline:
<ol>
  <li><strong>Step 1:</strong> Attempt fast <code>Regex-based</code> classification using hand-coded patterns.</li>
  <li><strong>Step 2:</strong> If unclassified, check training data size:
    <ul>
      <li>✅ If sufficient data → use fine-tuned <code>BERT + Logistic Regression</code></li>
      <li>❌ If insufficient data → send the sample to <code>LLM</code> (e.g., LLaMA via Groq API)</li>
    </ul>
  </li>
</ol>
</p>

<p align="center">
  <img width="890" alt="image" src="https://github.com/user-attachments/assets/2b4db1d1-5f0f-44da-a861-5ad6ac4750d1" />
</p>

<p align="center"><em>Figure: Hybrid Log Classification Flow — Regex → BERT or LLM based on data availability</em></p>

<h2>🚀 Setup Instructions</h2>

<h3>1️⃣ Install Dependencies</h3>
<pre><code>pip install -r requirements.txt</code></pre>

<h3>2️⃣ Run the FastAPI Server</h3>
<pre><code>uvicorn server:app --reload</code></pre>

<h3>3️⃣ Access API</h3>
<ul>
  <li>Swagger UI: <a href="http://127.0.0.1:8000/docs">http://127.0.0.1:8000/docs</a></li>
  <li>ReDoc: <a href="http://127.0.0.1:8000/redoc">http://127.0.0.1:8000/redoc</a></li>
</ul>

<h2>📥 Usage Instructions</h2>
<p>Upload a CSV file to the <code>/classify/</code> endpoint with the following columns:</p>
<table border="1" cellspacing="0" cellpadding="6">
  <thead>
    <tr>
      <th>Column</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>source</code></td>
      <td>Source system name (e.g., LegacyCRM)</td>
    </tr>
    <tr>
      <td><code>log_message</code></td>
      <td>Raw log string</td>
    </tr>
  </tbody>
</table>

<h3>✅ Response</h3>
<p>Returns a CSV with an additional column <code>target_label</code> representing predicted log category (e.g., Security Alert, Workflow Error, etc.)</p>
<img width="1022" alt="Screenshot 2025-07-02 at 2 51 12 AM" src="https://github.com/user-attachments/assets/7123add9-2d98-44be-b770-78b03a774d73" />

