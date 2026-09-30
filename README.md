# Local Corrective RAG

A notebook-based RAG demo that searches a PDF, evaluates retrieved passages,
and uses web search when the PDF evidence is weak.

## Workflow

1. Load and split the PDF.
2. Create embeddings and a FAISS index.
3. Retrieve five chunks and score their relevance.
4. Select an evidence source:
   - CORRECT: PDF passages.
   - INCORRECT: web search.
   - AMBIGUOUS: PDF passages plus web search.
5. Filter sentences and generate an answer from the remaining context.

These verdicts describe retrieval quality, not verified answer accuracy.

## Technologies

- Python and Jupyter
- LangGraph and LangChain
- Ollama: granite4.1:3b
- Embeddings: nomic-embed-text:v1.5
- FAISS and PyPDFLoader
- Tavily web search

## Setup

Install Python and Ollama, then run:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt

ollama pull granite4.1:3b
ollama pull nomic-embed-text:v1.5
```

Create a local `.env` file:

```text
TAVILY_API_KEY=your_tavily_api_key
```

Ensure the notebook calls `load_dotenv()` before initializing Tavily.

Add your own text-based PDF as `intro-to-ml.pdf`, or update the loader path.
The PDF and API key are not included in this repository.

Open `corrective_rag.ipynb` in VS Code, select the `.venv` Python kernel,
and run cells from top to bottom. Ollama must be running.

Change the question in the final example cell to try another query.
Rebuilding the index is unnecessary for each new question.

## Implementation Details

- Chunk size: 1,000 characters
- Chunk overlap: 150 characters
- Retrieval: top 5 chunks
- Embedding batches: 8 chunks
- Relevance thresholds: 0.3 and 0.7
- Empty refined context returns a fallback response.
- Web-search errors are printed and return no web documents.

## Limitations

- Small local models can misjudge relevance or generate inaccurate answers.
- Sentence filtering can lose context or retain irrelevant text.
- Web search requires internet access and a Tavily API key.
- Web-search failure handling has not been tested with a simulated outage.
- PDF extraction does not include OCR.
- The FAISS index is held in memory and is not saved automatically.
- Fresh-environment installation has not been verified.
- This is a learning project, not a production-ready system.