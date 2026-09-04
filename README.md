# ResearchMind

ResearchMind is a Streamlit multi-agent research system. It uses Groq for language-model responses, Tavily for web search, and a web scraper for deeper reading.

The pipeline has four stages:

1. Search Agent finds recent web information.
2. Reader Agent selects and scrapes a relevant source.
3. Writer Chain creates a structured research report.
4. Critic Chain reviews and scores the report.

## Requirements

- Python 3.13 or newer
- Docker Desktop (optional)
- A Groq API key
- A Tavily API key

## Local Setup

Clone the repository and create a virtual environment:

```bash
git clone https://github.com/AkarshVyas/Multi-agent-research-system.git
cd Multi-agent-research-system
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key
```

Start the application:

```bash
./.venv/bin/python -m streamlit run app.py
```

Open the URL printed by Streamlit, usually `http://localhost:8501`.

## Docker

Build and start the application with Docker Compose:

```bash
docker compose up -d --build
```

The Compose configuration maps the application to:

```text
http://localhost:8502
```

Stop the container with:

```bash
docker compose down
```

The API keys are loaded from the local `.env` file at runtime. They are not included in the Docker image.

## Docker Hub Image

The published image is:

```text
myselfsatyam/researchmind:latest
```

Pull and run it directly:

```bash
docker pull myselfsatyam/researchmind:latest
docker run --env-file .env -p 8501:8501 myselfsatyam/researchmind:latest
```

Then open `http://localhost:8501`.

To publish a new version:

```bash
docker login
docker build -t myselfsatyam/researchmind:latest .
docker push myselfsatyam/researchmind:latest
```

## Project Structure

```text
.
├── agents.py             Agent and chain definitions
├── app.py                Streamlit user interface
├── pipeline.py           Command-line pipeline runner
├── tools.py              Tavily search and URL scraping tools
├── Dockerfile            Container image definition
├── docker-compose.yml    Local container orchestration
├── requirements.txt      Python dependencies
└── .env                  Local API keys, not committed
```

## Command-Line Pipeline

The pipeline can also be run without Streamlit:

```bash
./.venv/bin/python pipeline.py
```

## Troubleshooting

### Port 8501 is already in use

Use Docker Compose on port 8502, or stop the local Streamlit process:

```bash
docker compose up -d
```

### Groq model errors

The application currently uses `openai/gpt-oss-120b`. Confirm that your Groq account and API key can access this model.

### API quota or authentication errors

Check that both variables are present in `.env` and that the keys are active:

```bash
GROQ_API_KEY=...
TAVILY_API_KEY=...
```

Never commit `.env` or publish API keys in source code, Dockerfiles, or images.
