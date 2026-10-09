# Syllabus Quiz Studio

A Streamlit application that uses an Ollama endpoint to create printable multiple-choice quizzes from a supplied syllabus.

## Setup

From this directory, create and activate the virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

For local development, set the Ollama credentials in the project `.env` file:

```dotenv
OLLAMA_API_KEY=YOUR_API_KEY
OLLAMA_BASE_URL=https://your-ollama-endpoint.com
OLLAMA_MODEL=llama3.2
```

Replace the example API key and endpoint with your authenticated Ollama service values. The app sends the API key as a Bearer token.

Install [Ollama](https://ollama.com/), start its local service, and pull a model, for example:

```bash
ollama pull llama3.2
```

## Deploy on Streamlit Community Cloud

1. Push this repository to GitHub and create an app at [share.streamlit.io](https://share.streamlit.io/).
2. Select this repository and the `main` branch. Set **Main file path** to `llm project/app.py`.
3. In the app's **Settings → Secrets**, add the following TOML values using your real, internet-accessible Ollama service settings:

   ```toml
   OLLAMA_API_KEY = "your-rotated-api-key"
   OLLAMA_BASE_URL = "https://your-ollama-endpoint.com"
   OLLAMA_MODEL = "llama3.2"
   ```

   Keep secrets out of GitHub and never commit a `.env` file. Streamlit Cloud cannot connect to an Ollama server running only on your computer; use an Ollama endpoint reachable from the deployed app.
4. Save the secrets and deploy or reboot the app.

The app reads Streamlit secrets on Cloud and falls back to the local `.env` file/environment when no Streamlit secrets file is configured.

## Run

```bash
streamlit run app.py
```

Paste a syllabus or upload a `.txt`/`.md` syllabus, configure the local Ollama URL and model, then generate and download a PDF or Markdown quiz. Generation requires an exact syllabus quotation for every question; questions failing that check are rejected.

No quiz template was present in the source repository when this project was created, so the PDF uses a standard layout: heading, numbered questions, A-D options, difficulty, marks, and a separate evidence-backed answer key.