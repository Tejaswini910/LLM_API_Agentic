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
2. Select this repository and the `main` branch. Set **Main file path** to `llm_project/app.py`. Set the dependency file to `llm_project/requirements.txt` (or use the repository-root `requirements.txt`). These paths intentionally contain no spaces so Streamlit Cloud can pass them to its package installer correctly.
3. In the app's **Settings → Secrets**, add the following TOML values using your real, internet-accessible Ollama service settings:

   ```toml
   OLLAMA_API_KEY = "your-rotated-api-key"
   OLLAMA_BASE_URL = "https://ollama.com/api"
   OLLAMA_MODEL = "gemma4:31b"
   ```

   Keep secrets out of GitHub and never commit a `.env` file. Streamlit Cloud cannot connect to an Ollama server running only on your computer; use an Ollama endpoint reachable from the deployed app.
4. Save the secrets and deploy or reboot the app.

The app reads Streamlit secrets on Cloud and falls back to the local `.env` file/environment when no Streamlit secrets file is configured.

## Run locally

```bash
streamlit run app.py
```

Paste the syllabus, choose **Easy**, **Medium**, or **Hard**, set the number of questions (1–50), then click **Generate quiz**. The app requests exactly the selected number of multiple-choice questions at the selected difficulty and offers PDF, Markdown, and JSON downloads. It returns fewer only when the syllabus cannot support more distinct questions without inventing facts, and tells the user when that happens. It does not require a file upload or Excel template.

The model is instructed to use only syllabus information and return a JSON array with `question`, `option_a`, `option_b`, `option_c`, `option_d`, and `answer` fields. The app validates the JSON shape and removes duplicate questions. Ollama Cloud does not support its structured-output format option, so the app requests JSON in the prompt and validates the result. If a response is cut off, shorten the syllabus and try again.

No quiz template was present in the source repository when this project was created, so the PDF uses a standard layout with numbered questions, A-D options, and a separate answer key.