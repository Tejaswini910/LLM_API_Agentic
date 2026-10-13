# Syllabus Quiz Studio

A Streamlit application that uses an Ollama endpoint to create printable multiple-choice quizzes from a supplied syllabus.

## Setup

From this directory, create and activate the virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

For Ollama Cloud, set the API key, API base URL, and a model available in the hosted model catalog in the project `.env` file:

```dotenv
OLLAMA_API_KEY=YOUR_API_KEY
OLLAMA_BASE_URL=https://ollama.com/api
OLLAMA_MODEL=gemma4:31b
```

Replace the example API key and endpoint with your authenticated Ollama service values. The app sends the API key as a Bearer token.

The Ollama Cloud `OLLAMA_MODEL` must be listed in the [hosted model catalog](https://ollama.com/api/tags). A local-only model such as `llama3.2` is not automatically available through Ollama Cloud.

For local development with an Ollama server instead of Ollama Cloud, set `OLLAMA_BASE_URL=http://localhost:11434`, install [Ollama](https://ollama.com/), start its local service, and pull a model, for example:

```bash
ollama pull llama3.2
```

## Deploy on Streamlit Community Cloud

1. Push this repository to GitHub and create an app at [share.streamlit.io](https://share.streamlit.io/).
2. Select this repository and the `main` branch. Set **Main file path** to `llm project/app.py`. Streamlit Cloud installs dependencies from the repository-root `requirements.txt`.
3. In the app's **Settings → Secrets**, add the following TOML values using your real, internet-accessible Ollama service settings:

   ```toml
   OLLAMA_API_KEY = "your-rotated-api-key"
   OLLAMA_BASE_URL = "https://ollama.com/api"
   OLLAMA_MODEL = "gemma4:31b"
   ```

   Keep secrets out of GitHub and never commit a `.env` file. Streamlit Cloud cannot connect to an Ollama server running only on your computer; use an Ollama endpoint reachable from the deployed app.
4. Save the secrets and deploy or reboot the app.

The app reads Streamlit secrets on Cloud and falls back to the local `.env` file/environment when no Streamlit secrets file is configured.

## Run

```bash
streamlit run app.py
```

Paste a syllabus or upload a `.txt`/`.md` syllabus, configure the Ollama URL and a model available at that endpoint, then generate and download a PDF or Markdown quiz. Generation requires an exact syllabus quotation for every question; questions failing that check are rejected. Ollama Cloud does not support its structured-output format option, so the app requests JSON in the prompt and validates the result. If a response is cut off, reduce the question count or shorten the syllabus and try again.

No quiz template was present in the source repository when this project was created, so the PDF uses a standard layout: heading, numbered questions, A-D options, difficulty, marks, and a separate evidence-backed answer key.