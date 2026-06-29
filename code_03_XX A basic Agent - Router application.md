### 03.03. Setting up Indexes


```python
#Install prerequisite packages
!pip install python-dotenv==1.0.0

!pip install llama-index==0.10.59
!pip install llama-index-core==0.10.59
!pip install llama-index-llms-gemini==0.1.11
!pip install llama-index-embeddings-gemini==0.1.8

```


```python
#Setup Gemini connection
from dotenv import load_dotenv
from llama_index.llms.gemini import Gemini
from llama_index.embeddings.gemini import GeminiEmbedding
from llama_index.core import Settings
from google.api_core.exceptions import ResourceExhausted
import os
import nest_asyncio
import re
import time

nest_asyncio.apply()
load_dotenv()

# Gemini API key. Set this in your environment or in a local .env file:
# GEMINI_API_KEY=your_api_key_here
gemini_api_key = os.getenv("GEMINI_API_KEY") or os.getenv("GOOGLE_API_KEY")
if not gemini_api_key:
    raise ValueError("Set GEMINI_API_KEY in your environment or local .env file before running this notebook.")

# The LlamaIndex Gemini integration reads GOOGLE_API_KEY internally.
os.environ["GOOGLE_API_KEY"] = gemini_api_key

gemini_model = os.getenv("GEMINI_MODEL", "models/gemini-3.5-flash")
if not (gemini_model.startswith("models/") or gemini_model.startswith("tunedModels/")):
    gemini_model = f"models/{gemini_model}"

gemini_embed_model = os.getenv("GEMINI_EMBED_MODEL", "gemini-embedding-2")
if not gemini_embed_model.startswith("models/"):
    gemini_embed_model = f"models/{gemini_embed_model}"

is_embedding_2_model = "gemini-embedding-2" in gemini_embed_model

_gemini_last_request_at = 0.0


def _wait_for_gemini_rate_limit():
    global _gemini_last_request_at
    min_interval = float(os.getenv("GEMINI_MIN_REQUEST_INTERVAL_SECONDS", "13"))
    elapsed = time.monotonic() - _gemini_last_request_at
    wait_time = min_interval - elapsed
    if wait_time > 0:
        time.sleep(wait_time)
    _gemini_last_request_at = time.monotonic()


def _quota_retry_delay_seconds(error):
    message = str(error)
    retry_match = re.search(r"Please retry in ([0-9.]+)s", message)
    if retry_match:
        return float(retry_match.group(1)) + 2
    retry_delay_match = re.search(r"retry_delay\\s*\\{\\s*seconds:\\s*(\\d+)", message)
    if retry_delay_match:
        return float(retry_delay_match.group(1)) + 2
    return 60.0


class RateLimitedGemini(Gemini):
    def _call_with_quota_retry(self, call):
        max_retries = int(os.getenv("GEMINI_QUOTA_RETRIES", "2"))
        for attempt in range(max_retries + 1):
            _wait_for_gemini_rate_limit()
            try:
                return call()
            except ResourceExhausted as error:
                if attempt >= max_retries:
                    raise
                wait_time = _quota_retry_delay_seconds(error)
                print(f"Gemini quota reached. Waiting {wait_time:.1f}s before retrying...")
                time.sleep(wait_time)

    def chat(self, messages, **kwargs):
        return self._call_with_quota_retry(lambda: super(RateLimitedGemini, self).chat(messages, **kwargs))

    def complete(self, prompt, formatted=False, **kwargs):
        return self._call_with_quota_retry(lambda: super(RateLimitedGemini, self).complete(prompt, formatted=formatted, **kwargs))


#Setup the LLM
Settings.llm = RateLimitedGemini(
    model=gemini_model,
    api_key=gemini_api_key,
    temperature=0.1,
)

#Setup the embedding model for RAG
Settings.embed_model = GeminiEmbedding(
    model_name=gemini_embed_model,
    api_key=gemini_api_key,
    task_type=None if is_embedding_2_model else "retrieval_document",
)

```

    /usr/local/python/3.12.1/lib/python3.12/site-packages/tqdm/auto.py:21: TqdmWarning: IProgress not found. Please update jupyter and ipywidgets. See https://ipywidgets.readthedocs.io/en/stable/user_install.html
      from .autonotebook import tqdm as notebook_tqdm



```python
#Create indexes for vector search
from llama_index.core import SimpleDirectoryReader
from llama_index.core.node_parser import SentenceSplitter
from llama_index.core import  VectorStoreIndex

splitter=SentenceSplitter(chunk_size=1024)

#-------------------------------------------------------------------
#Setup Aeroflow document index
#-------------------------------------------------------------------
aeroflow_documents=SimpleDirectoryReader(
    input_files=["AeroFlow_Specification_Document.pdf"])\
            .load_data()

#Read documents into nodes
aeroflow_nodes=splitter.get_nodes_from_documents(aeroflow_documents)
#Create a vector Store
aeroflow_index=VectorStoreIndex(aeroflow_nodes)
#Create a query engine
aeroflow_query_engine = aeroflow_index.as_query_engine()

#-------------------------------------------------------------------
#Setup EchoSprint document index
#-------------------------------------------------------------------
ecosprint_documents=SimpleDirectoryReader(
    input_files=["EcoSprint_Specification_Document.pdf"])\
            .load_data()
#Read documents into nodes
ecosprint_nodes=splitter.get_nodes_from_documents(ecosprint_documents)
#Create a vector Store
ecosprint_index=VectorStoreIndex(ecosprint_nodes)
#Create a query engine
ecosprint_query_engine = ecosprint_index.as_query_engine()

```

### 03.04. Setup the Agentic Router


```python
from llama_index.core.tools import QueryEngineTool
from llama_index.core.query_engine.router_query_engine import RouterQueryEngine
from llama_index.core.selectors import LLMSingleSelector

#Create a query engine Tool for NoSQL
aeroflow_tool = QueryEngineTool.from_defaults(
    query_engine=aeroflow_query_engine,
    name="Aeroflow specifications",
    description=(
        "Contains information about Aeroflow : Design, features, technology, maintenance, warranty"
    ),
)

#Create a query engine Tool for NLP
ecosprint_tool = QueryEngineTool.from_defaults(
    query_engine=ecosprint_query_engine,
    name="EcoSprint specifications",
    description=(
        "Contains information about EcoSprint : Design, features, technology, maintenance, warranty"
    ),
)

#Create a Router Agent. Provide the Tools to the Agent
router_agent=RouterQueryEngine(
    selector=LLMSingleSelector.from_defaults(),
    query_engine_tools=[
        aeroflow_tool,
        ecosprint_tool,
    ],
    verbose=True
)
```

### 03.05. Route with Agentic AI


```python
#Ask a question about NoSQL
response = router_agent.query("What colors are available for AeroFlow?")
print("\nResponse: ",str(response))
```

    [1;3;38;5;200mSelecting query engine 0: Choice (1) explicitly states that it 'Contains information about Aeroflow', which is the subject of the question. Choice (2) contains information about 'EcoSprint', which is not relevant to the question about 'AeroFlow'..
    [0m
    Response:  The AeroFlow is available in Coastal Blue, Sunset Orange, and Pearl White.



```python
response = router_agent.query("What colors are available for EcoSprint?")
print("\nResponse: ",str(response))
```

    [1;3;38;5;200mSelecting query engine 1: Choice (2) explicitly states that it 'Contains information about EcoSprint'. The question 'What colors are available for EcoSprint?' is specifically about EcoSprint, and information regarding available colors would typically fall under 'Design' or 'features' which are listed as categories within choice (2)..
    [0m
    Response:  The EcoSprint is available in Midnight Black, Ocean Blue, and Pearl White.



```python

```
