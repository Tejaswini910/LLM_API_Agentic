# Syllabus Quiz Studio

A small Streamlit application that uses a local Ollama model to create printable multiple-choice quizzes from a supplied syllabus.

## Setup

From this directory, create and activate the virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Set the Ollama credentials in the project `.env` file:

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

## Run

```bash
streamlit run app.py
```

Paste a syllabus or upload a `.txt`/`.md` syllabus, configure the local Ollama URL and model, then generate and download a PDF or Markdown quiz. Generation requires an exact syllabus quotation for every question; questions failing that check are rejected.

No quiz template was present in the source repository when this project was created, so the PDF uses a standard layout: heading, numbered questions, A-D options, difficulty, marks, and a separate evidence-backed answer key.