### 06.03. Setup functions and indexes


```python
#Install prerequisite packages
!pip install python-dotenv==1.0.0

!pip install llama-index==0.10.59
!pip install llama-index-core==0.10.59
!pip install llama-index-llms-gemini==0.1.11
!pip install llama-index-embeddings-gemini==0.1.8

```

    Requirement already satisfied: python-dotenv==1.0.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (1.0.0)
    
    [1m[[0m[34;49mnotice[0m[1;39;49m][0m[39;49m A new release of pip is available: [0m[31;49m26.0.1[0m[39;49m -> [0m[32;49m26.1.2[0m
    [1m[[0m[34;49mnotice[0m[1;39;49m][0m[39;49m To update, run: [0m[32;49mpython3 -m pip install --upgrade pip[0m
    Requirement already satisfied: llama-index==0.10.59 in /usr/local/python/3.12.1/lib/python3.12/site-packages (0.10.59)
    Requirement already satisfied: llama-index-agent-openai<0.3.0,>=0.1.4 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index==0.10.59) (0.2.9)
    Requirement already satisfied: llama-index-cli<0.2.0,>=0.1.2 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index==0.10.59) (0.1.13)
    Requirement already satisfied: llama-index-core==0.10.59 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index==0.10.59) (0.10.59)
    Requirement already satisfied: llama-index-embeddings-openai<0.2.0,>=0.1.5 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index==0.10.59) (0.1.11)
    Requirement already satisfied: llama-index-indices-managed-llama-cloud>=0.2.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index==0.10.59) (0.2.7)
    Requirement already satisfied: llama-index-legacy<0.10.0,>=0.9.48 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index==0.10.59) (0.9.48.post4)
    Requirement already satisfied: llama-index-llms-openai<0.2.0,>=0.1.27 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index==0.10.59) (0.1.27)
    Requirement already satisfied: llama-index-multi-modal-llms-openai<0.2.0,>=0.1.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index==0.10.59) (0.1.9)
    Requirement already satisfied: llama-index-program-openai<0.2.0,>=0.1.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index==0.10.59) (0.1.7)
    Requirement already satisfied: llama-index-question-gen-openai<0.2.0,>=0.1.2 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index==0.10.59) (0.1.3)
    Requirement already satisfied: llama-index-readers-file<0.2.0,>=0.1.4 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index==0.10.59) (0.1.33)
    Requirement already satisfied: llama-index-readers-llama-parse>=0.1.2 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index==0.10.59) (0.1.6)
    Requirement already satisfied: PyYAML>=6.0.1 in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (6.0.3)
    Requirement already satisfied: SQLAlchemy>=1.4.49 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from SQLAlchemy[asyncio]>=1.4.49->llama-index-core==0.10.59->llama-index==0.10.59) (2.0.51)
    Requirement already satisfied: aiohttp<4.0.0,>=3.8.6 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (3.14.1)
    Requirement already satisfied: dataclasses-json in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (0.6.7)
    Requirement already satisfied: deprecated>=1.2.9.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (1.3.1)
    Requirement already satisfied: dirtyjson<2.0.0,>=1.0.8 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (1.0.8)
    Requirement already satisfied: fsspec>=2023.5.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (2026.6.0)
    Requirement already satisfied: httpx in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (0.28.1)
    Requirement already satisfied: nest-asyncio<2.0.0,>=1.5.8 in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (1.6.0)
    Requirement already satisfied: networkx>=3.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (3.6.1)
    Requirement already satisfied: nltk<4.0.0,>=3.8.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (3.9.4)
    Requirement already satisfied: numpy<2.0.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (1.26.4)
    Requirement already satisfied: openai>=1.1.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (1.109.1)
    Requirement already satisfied: pandas in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (3.0.4)
    Requirement already satisfied: pillow>=9.0.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (10.4.0)
    Requirement already satisfied: requests>=2.31.0 in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (2.32.5)
    Requirement already satisfied: tenacity!=8.4.0,<9.0.0,>=8.2.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (8.5.0)
    Requirement already satisfied: tiktoken>=0.3.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (0.13.0)
    Requirement already satisfied: tqdm<5.0.0,>=4.66.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (4.68.3)
    Requirement already satisfied: typing-extensions>=4.5.0 in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (4.15.0)
    Requirement already satisfied: typing-inspect>=0.8.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (0.9.0)
    Requirement already satisfied: wrapt in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59->llama-index==0.10.59) (2.2.2)
    Requirement already satisfied: aiohappyeyeballs>=2.5.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core==0.10.59->llama-index==0.10.59) (2.6.2)
    Requirement already satisfied: aiosignal>=1.4.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core==0.10.59->llama-index==0.10.59) (1.4.0)
    Requirement already satisfied: attrs>=17.3.0 in /home/codespace/.local/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core==0.10.59->llama-index==0.10.59) (25.4.0)
    Requirement already satisfied: frozenlist>=1.1.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core==0.10.59->llama-index==0.10.59) (1.8.0)
    Requirement already satisfied: multidict<7.0,>=4.5 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core==0.10.59->llama-index==0.10.59) (6.7.1)
    Requirement already satisfied: propcache>=0.2.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core==0.10.59->llama-index==0.10.59) (0.5.2)
    Requirement already satisfied: yarl<2.0,>=1.17.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core==0.10.59->llama-index==0.10.59) (1.24.2)
    Requirement already satisfied: beautifulsoup4<5.0.0,>=4.12.3 in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-readers-file<0.2.0,>=0.1.4->llama-index==0.10.59) (4.14.3)
    Requirement already satisfied: pypdf<5.0.0,>=4.0.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-readers-file<0.2.0,>=0.1.4->llama-index==0.10.59) (4.3.1)
    Requirement already satisfied: striprtf<0.0.27,>=0.0.26 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-readers-file<0.2.0,>=0.1.4->llama-index==0.10.59) (0.0.26)
    Requirement already satisfied: soupsieve>=1.6.1 in /home/codespace/.local/lib/python3.12/site-packages (from beautifulsoup4<5.0.0,>=4.12.3->llama-index-readers-file<0.2.0,>=0.1.4->llama-index==0.10.59) (2.8.3)
    Requirement already satisfied: click in /usr/local/python/3.12.1/lib/python3.12/site-packages (from nltk<4.0.0,>=3.8.1->llama-index-core==0.10.59->llama-index==0.10.59) (8.4.2)
    Requirement already satisfied: joblib in /usr/local/python/3.12.1/lib/python3.12/site-packages (from nltk<4.0.0,>=3.8.1->llama-index-core==0.10.59->llama-index==0.10.59) (1.5.3)
    Requirement already satisfied: regex>=2021.8.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from nltk<4.0.0,>=3.8.1->llama-index-core==0.10.59->llama-index==0.10.59) (2026.6.28)
    Requirement already satisfied: idna>=2.0 in /home/codespace/.local/lib/python3.12/site-packages (from yarl<2.0,>=1.17.0->aiohttp<4.0.0,>=3.8.6->llama-index-core==0.10.59->llama-index==0.10.59) (3.11)
    Requirement already satisfied: llama-cloud>=0.0.11 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-indices-managed-llama-cloud>=0.2.0->llama-index==0.10.59) (2.9.1)
    Requirement already satisfied: anyio<5,>=3.5.0 in /home/codespace/.local/lib/python3.12/site-packages (from llama-cloud>=0.0.11->llama-index-indices-managed-llama-cloud>=0.2.0->llama-index==0.10.59) (4.12.1)
    Requirement already satisfied: distro<2,>=1.7.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-cloud>=0.0.11->llama-index-indices-managed-llama-cloud>=0.2.0->llama-index==0.10.59) (1.9.0)
    Requirement already satisfied: pydantic<3,>=1.9.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-cloud>=0.0.11->llama-index-indices-managed-llama-cloud>=0.2.0->llama-index==0.10.59) (2.13.4)
    Requirement already satisfied: sniffio in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-cloud>=0.0.11->llama-index-indices-managed-llama-cloud>=0.2.0->llama-index==0.10.59) (1.3.1)
    Requirement already satisfied: certifi in /home/codespace/.local/lib/python3.12/site-packages (from httpx->llama-index-core==0.10.59->llama-index==0.10.59) (2026.2.25)
    Requirement already satisfied: httpcore==1.* in /home/codespace/.local/lib/python3.12/site-packages (from httpx->llama-index-core==0.10.59->llama-index==0.10.59) (1.0.9)
    Requirement already satisfied: h11>=0.16 in /home/codespace/.local/lib/python3.12/site-packages (from httpcore==1.*->httpx->llama-index-core==0.10.59->llama-index==0.10.59) (0.16.0)
    Requirement already satisfied: annotated-types>=0.6.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from pydantic<3,>=1.9.0->llama-cloud>=0.0.11->llama-index-indices-managed-llama-cloud>=0.2.0->llama-index==0.10.59) (0.7.0)
    Requirement already satisfied: pydantic-core==2.46.4 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from pydantic<3,>=1.9.0->llama-cloud>=0.0.11->llama-index-indices-managed-llama-cloud>=0.2.0->llama-index==0.10.59) (2.46.4)
    Requirement already satisfied: typing-inspection>=0.4.2 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from pydantic<3,>=1.9.0->llama-cloud>=0.0.11->llama-index-indices-managed-llama-cloud>=0.2.0->llama-index==0.10.59) (0.4.2)
    Requirement already satisfied: llama-parse>=0.4.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-readers-llama-parse>=0.1.2->llama-index==0.10.59) (0.4.9)
    Requirement already satisfied: jiter<1,>=0.4.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from openai>=1.1.0->llama-index-core==0.10.59->llama-index==0.10.59) (0.15.0)
    Requirement already satisfied: charset_normalizer<4,>=2 in /home/codespace/.local/lib/python3.12/site-packages (from requests>=2.31.0->llama-index-core==0.10.59->llama-index==0.10.59) (3.4.5)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /home/codespace/.local/lib/python3.12/site-packages (from requests>=2.31.0->llama-index-core==0.10.59->llama-index==0.10.59) (2.6.3)
    Requirement already satisfied: greenlet>=1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from SQLAlchemy>=1.4.49->SQLAlchemy[asyncio]>=1.4.49->llama-index-core==0.10.59->llama-index==0.10.59) (3.5.3)
    Requirement already satisfied: mypy-extensions>=0.3.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from typing-inspect>=0.8.0->llama-index-core==0.10.59->llama-index==0.10.59) (1.1.0)
    Requirement already satisfied: marshmallow<4.0.0,>=3.18.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from dataclasses-json->llama-index-core==0.10.59->llama-index==0.10.59) (3.26.2)
    Requirement already satisfied: packaging>=17.0 in /home/codespace/.local/lib/python3.12/site-packages (from marshmallow<4.0.0,>=3.18.0->dataclasses-json->llama-index-core==0.10.59->llama-index==0.10.59) (26.0)
    Requirement already satisfied: python-dateutil>=2.8.2 in /home/codespace/.local/lib/python3.12/site-packages (from pandas->llama-index-core==0.10.59->llama-index==0.10.59) (2.9.0.post0)
    Requirement already satisfied: six>=1.5 in /home/codespace/.local/lib/python3.12/site-packages (from python-dateutil>=2.8.2->pandas->llama-index-core==0.10.59->llama-index==0.10.59) (1.17.0)
    
    [1m[[0m[34;49mnotice[0m[1;39;49m][0m[39;49m A new release of pip is available: [0m[31;49m26.0.1[0m[39;49m -> [0m[32;49m26.1.2[0m
    [1m[[0m[34;49mnotice[0m[1;39;49m][0m[39;49m To update, run: [0m[32;49mpython3 -m pip install --upgrade pip[0m
    Requirement already satisfied: llama-index-core==0.10.59 in /usr/local/python/3.12.1/lib/python3.12/site-packages (0.10.59)
    Requirement already satisfied: PyYAML>=6.0.1 in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-core==0.10.59) (6.0.3)
    Requirement already satisfied: SQLAlchemy>=1.4.49 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from SQLAlchemy[asyncio]>=1.4.49->llama-index-core==0.10.59) (2.0.51)
    Requirement already satisfied: aiohttp<4.0.0,>=3.8.6 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59) (3.14.1)
    Requirement already satisfied: dataclasses-json in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59) (0.6.7)
    Requirement already satisfied: deprecated>=1.2.9.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59) (1.3.1)
    Requirement already satisfied: dirtyjson<2.0.0,>=1.0.8 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59) (1.0.8)
    Requirement already satisfied: fsspec>=2023.5.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59) (2026.6.0)
    Requirement already satisfied: httpx in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-core==0.10.59) (0.28.1)
    Requirement already satisfied: nest-asyncio<2.0.0,>=1.5.8 in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-core==0.10.59) (1.6.0)
    Requirement already satisfied: networkx>=3.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59) (3.6.1)
    Requirement already satisfied: nltk<4.0.0,>=3.8.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59) (3.9.4)
    Requirement already satisfied: numpy<2.0.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59) (1.26.4)
    Requirement already satisfied: openai>=1.1.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59) (1.109.1)
    Requirement already satisfied: pandas in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59) (3.0.4)
    Requirement already satisfied: pillow>=9.0.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59) (10.4.0)
    Requirement already satisfied: requests>=2.31.0 in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-core==0.10.59) (2.32.5)
    Requirement already satisfied: tenacity!=8.4.0,<9.0.0,>=8.2.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59) (8.5.0)
    Requirement already satisfied: tiktoken>=0.3.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59) (0.13.0)
    Requirement already satisfied: tqdm<5.0.0,>=4.66.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59) (4.68.3)
    Requirement already satisfied: typing-extensions>=4.5.0 in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-core==0.10.59) (4.15.0)
    Requirement already satisfied: typing-inspect>=0.8.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59) (0.9.0)
    Requirement already satisfied: wrapt in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core==0.10.59) (2.2.2)
    Requirement already satisfied: aiohappyeyeballs>=2.5.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core==0.10.59) (2.6.2)
    Requirement already satisfied: aiosignal>=1.4.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core==0.10.59) (1.4.0)
    Requirement already satisfied: attrs>=17.3.0 in /home/codespace/.local/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core==0.10.59) (25.4.0)
    Requirement already satisfied: frozenlist>=1.1.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core==0.10.59) (1.8.0)
    Requirement already satisfied: multidict<7.0,>=4.5 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core==0.10.59) (6.7.1)
    Requirement already satisfied: propcache>=0.2.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core==0.10.59) (0.5.2)
    Requirement already satisfied: yarl<2.0,>=1.17.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core==0.10.59) (1.24.2)
    Requirement already satisfied: click in /usr/local/python/3.12.1/lib/python3.12/site-packages (from nltk<4.0.0,>=3.8.1->llama-index-core==0.10.59) (8.4.2)
    Requirement already satisfied: joblib in /usr/local/python/3.12.1/lib/python3.12/site-packages (from nltk<4.0.0,>=3.8.1->llama-index-core==0.10.59) (1.5.3)
    Requirement already satisfied: regex>=2021.8.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from nltk<4.0.0,>=3.8.1->llama-index-core==0.10.59) (2026.6.28)
    Requirement already satisfied: idna>=2.0 in /home/codespace/.local/lib/python3.12/site-packages (from yarl<2.0,>=1.17.0->aiohttp<4.0.0,>=3.8.6->llama-index-core==0.10.59) (3.11)
    Requirement already satisfied: anyio<5,>=3.5.0 in /home/codespace/.local/lib/python3.12/site-packages (from openai>=1.1.0->llama-index-core==0.10.59) (4.12.1)
    Requirement already satisfied: distro<2,>=1.7.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from openai>=1.1.0->llama-index-core==0.10.59) (1.9.0)
    Requirement already satisfied: jiter<1,>=0.4.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from openai>=1.1.0->llama-index-core==0.10.59) (0.15.0)
    Requirement already satisfied: pydantic<3,>=1.9.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from openai>=1.1.0->llama-index-core==0.10.59) (2.13.4)
    Requirement already satisfied: sniffio in /usr/local/python/3.12.1/lib/python3.12/site-packages (from openai>=1.1.0->llama-index-core==0.10.59) (1.3.1)
    Requirement already satisfied: certifi in /home/codespace/.local/lib/python3.12/site-packages (from httpx->llama-index-core==0.10.59) (2026.2.25)
    Requirement already satisfied: httpcore==1.* in /home/codespace/.local/lib/python3.12/site-packages (from httpx->llama-index-core==0.10.59) (1.0.9)
    Requirement already satisfied: h11>=0.16 in /home/codespace/.local/lib/python3.12/site-packages (from httpcore==1.*->httpx->llama-index-core==0.10.59) (0.16.0)
    Requirement already satisfied: annotated-types>=0.6.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from pydantic<3,>=1.9.0->openai>=1.1.0->llama-index-core==0.10.59) (0.7.0)
    Requirement already satisfied: pydantic-core==2.46.4 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from pydantic<3,>=1.9.0->openai>=1.1.0->llama-index-core==0.10.59) (2.46.4)
    Requirement already satisfied: typing-inspection>=0.4.2 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from pydantic<3,>=1.9.0->openai>=1.1.0->llama-index-core==0.10.59) (0.4.2)
    Requirement already satisfied: charset_normalizer<4,>=2 in /home/codespace/.local/lib/python3.12/site-packages (from requests>=2.31.0->llama-index-core==0.10.59) (3.4.5)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /home/codespace/.local/lib/python3.12/site-packages (from requests>=2.31.0->llama-index-core==0.10.59) (2.6.3)
    Requirement already satisfied: greenlet>=1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from SQLAlchemy>=1.4.49->SQLAlchemy[asyncio]>=1.4.49->llama-index-core==0.10.59) (3.5.3)
    Requirement already satisfied: mypy-extensions>=0.3.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from typing-inspect>=0.8.0->llama-index-core==0.10.59) (1.1.0)
    Requirement already satisfied: marshmallow<4.0.0,>=3.18.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from dataclasses-json->llama-index-core==0.10.59) (3.26.2)
    Requirement already satisfied: packaging>=17.0 in /home/codespace/.local/lib/python3.12/site-packages (from marshmallow<4.0.0,>=3.18.0->dataclasses-json->llama-index-core==0.10.59) (26.0)
    Requirement already satisfied: python-dateutil>=2.8.2 in /home/codespace/.local/lib/python3.12/site-packages (from pandas->llama-index-core==0.10.59) (2.9.0.post0)
    Requirement already satisfied: six>=1.5 in /home/codespace/.local/lib/python3.12/site-packages (from python-dateutil>=2.8.2->pandas->llama-index-core==0.10.59) (1.17.0)
    
    [1m[[0m[34;49mnotice[0m[1;39;49m][0m[39;49m A new release of pip is available: [0m[31;49m26.0.1[0m[39;49m -> [0m[32;49m26.1.2[0m
    [1m[[0m[34;49mnotice[0m[1;39;49m][0m[39;49m To update, run: [0m[32;49mpython3 -m pip install --upgrade pip[0m
    Requirement already satisfied: llama-index-llms-gemini==0.1.11 in /usr/local/python/3.12.1/lib/python3.12/site-packages (0.1.11)
    Requirement already satisfied: google-generativeai<0.6.0,>=0.5.2 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-llms-gemini==0.1.11) (0.5.4)
    Requirement already satisfied: llama-index-core<0.11.0,>=0.10.11.post1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-llms-gemini==0.1.11) (0.10.59)
    Requirement already satisfied: pillow<11.0.0,>=10.2.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-llms-gemini==0.1.11) (10.4.0)
    Requirement already satisfied: google-ai-generativelanguage==0.6.4 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (0.6.4)
    Requirement already satisfied: google-api-core in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (2.30.3)
    Requirement already satisfied: google-api-python-client in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (2.198.0)
    Requirement already satisfied: google-auth>=2.15.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (2.55.1)
    Requirement already satisfied: protobuf in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (4.25.9)
    Requirement already satisfied: pydantic in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (2.13.4)
    Requirement already satisfied: tqdm in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (4.68.3)
    Requirement already satisfied: typing-extensions in /home/codespace/.local/lib/python3.12/site-packages (from google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (4.15.0)
    Requirement already satisfied: proto-plus<2.0.0dev,>=1.22.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-ai-generativelanguage==0.6.4->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (1.28.0)
    Requirement already satisfied: googleapis-common-protos<2.0.0,>=1.63.2 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-api-core->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (1.75.0)
    Requirement already satisfied: requests<3.0.0,>=2.20.0 in /home/codespace/.local/lib/python3.12/site-packages (from google-api-core->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (2.32.5)
    Requirement already satisfied: grpcio<2.0.0,>=1.33.2 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-api-core[grpc]!=2.0.*,!=2.1.*,!=2.10.*,!=2.2.*,!=2.3.*,!=2.4.*,!=2.5.*,!=2.6.*,!=2.7.*,!=2.8.*,!=2.9.*,<3.0.0dev,>=1.34.1->google-ai-generativelanguage==0.6.4->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (1.81.1)
    Requirement already satisfied: grpcio-status<2.0.0,>=1.33.2 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-api-core[grpc]!=2.0.*,!=2.1.*,!=2.10.*,!=2.2.*,!=2.3.*,!=2.4.*,!=2.5.*,!=2.6.*,!=2.7.*,!=2.8.*,!=2.9.*,<3.0.0dev,>=1.34.1->google-ai-generativelanguage==0.6.4->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (1.62.3)
    Requirement already satisfied: pyasn1-modules>=0.2.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-auth>=2.15.0->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (0.4.2)
    Requirement already satisfied: cryptography>=38.0.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-auth>=2.15.0->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (49.0.0)
    Requirement already satisfied: PyYAML>=6.0.1 in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (6.0.3)
    Requirement already satisfied: SQLAlchemy>=1.4.49 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from SQLAlchemy[asyncio]>=1.4.49->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (2.0.51)
    Requirement already satisfied: aiohttp<4.0.0,>=3.8.6 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (3.14.1)
    Requirement already satisfied: dataclasses-json in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (0.6.7)
    Requirement already satisfied: deprecated>=1.2.9.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (1.3.1)
    Requirement already satisfied: dirtyjson<2.0.0,>=1.0.8 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (1.0.8)
    Requirement already satisfied: fsspec>=2023.5.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (2026.6.0)
    Requirement already satisfied: httpx in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (0.28.1)
    Requirement already satisfied: nest-asyncio<2.0.0,>=1.5.8 in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (1.6.0)
    Requirement already satisfied: networkx>=3.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (3.6.1)
    Requirement already satisfied: nltk<4.0.0,>=3.8.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (3.9.4)
    Requirement already satisfied: numpy<2.0.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (1.26.4)
    Requirement already satisfied: openai>=1.1.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (1.109.1)
    Requirement already satisfied: pandas in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (3.0.4)
    Requirement already satisfied: tenacity!=8.4.0,<9.0.0,>=8.2.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (8.5.0)
    Requirement already satisfied: tiktoken>=0.3.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (0.13.0)
    Requirement already satisfied: typing-inspect>=0.8.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (0.9.0)
    Requirement already satisfied: wrapt in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (2.2.2)
    Requirement already satisfied: aiohappyeyeballs>=2.5.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (2.6.2)
    Requirement already satisfied: aiosignal>=1.4.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (1.4.0)
    Requirement already satisfied: attrs>=17.3.0 in /home/codespace/.local/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (25.4.0)
    Requirement already satisfied: frozenlist>=1.1.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (1.8.0)
    Requirement already satisfied: multidict<7.0,>=4.5 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (6.7.1)
    Requirement already satisfied: propcache>=0.2.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (0.5.2)
    Requirement already satisfied: yarl<2.0,>=1.17.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (1.24.2)
    Requirement already satisfied: click in /usr/local/python/3.12.1/lib/python3.12/site-packages (from nltk<4.0.0,>=3.8.1->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (8.4.2)
    Requirement already satisfied: joblib in /usr/local/python/3.12.1/lib/python3.12/site-packages (from nltk<4.0.0,>=3.8.1->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (1.5.3)
    Requirement already satisfied: regex>=2021.8.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from nltk<4.0.0,>=3.8.1->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (2026.6.28)
    Requirement already satisfied: charset_normalizer<4,>=2 in /home/codespace/.local/lib/python3.12/site-packages (from requests<3.0.0,>=2.20.0->google-api-core->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (3.4.5)
    Requirement already satisfied: idna<4,>=2.5 in /home/codespace/.local/lib/python3.12/site-packages (from requests<3.0.0,>=2.20.0->google-api-core->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (3.11)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /home/codespace/.local/lib/python3.12/site-packages (from requests<3.0.0,>=2.20.0->google-api-core->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (2.6.3)
    Requirement already satisfied: certifi>=2017.4.17 in /home/codespace/.local/lib/python3.12/site-packages (from requests<3.0.0,>=2.20.0->google-api-core->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (2026.2.25)
    Requirement already satisfied: cffi>=2.0.0 in /home/codespace/.local/lib/python3.12/site-packages (from cryptography>=38.0.3->google-auth>=2.15.0->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (2.0.0)
    Requirement already satisfied: pycparser in /home/codespace/.local/lib/python3.12/site-packages (from cffi>=2.0.0->cryptography>=38.0.3->google-auth>=2.15.0->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (3.0)
    Requirement already satisfied: anyio<5,>=3.5.0 in /home/codespace/.local/lib/python3.12/site-packages (from openai>=1.1.0->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (4.12.1)
    Requirement already satisfied: distro<2,>=1.7.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from openai>=1.1.0->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (1.9.0)
    Requirement already satisfied: jiter<1,>=0.4.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from openai>=1.1.0->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (0.15.0)
    Requirement already satisfied: sniffio in /usr/local/python/3.12.1/lib/python3.12/site-packages (from openai>=1.1.0->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (1.3.1)
    Requirement already satisfied: httpcore==1.* in /home/codespace/.local/lib/python3.12/site-packages (from httpx->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (1.0.9)
    Requirement already satisfied: h11>=0.16 in /home/codespace/.local/lib/python3.12/site-packages (from httpcore==1.*->httpx->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (0.16.0)
    Requirement already satisfied: annotated-types>=0.6.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from pydantic->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (0.7.0)
    Requirement already satisfied: pydantic-core==2.46.4 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from pydantic->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (2.46.4)
    Requirement already satisfied: typing-inspection>=0.4.2 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from pydantic->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (0.4.2)
    Requirement already satisfied: pyasn1<0.7.0,>=0.6.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from pyasn1-modules>=0.2.1->google-auth>=2.15.0->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (0.6.3)
    Requirement already satisfied: greenlet>=1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from SQLAlchemy>=1.4.49->SQLAlchemy[asyncio]>=1.4.49->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (3.5.3)
    Requirement already satisfied: mypy-extensions>=0.3.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from typing-inspect>=0.8.0->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (1.1.0)
    Requirement already satisfied: marshmallow<4.0.0,>=3.18.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from dataclasses-json->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (3.26.2)
    Requirement already satisfied: packaging>=17.0 in /home/codespace/.local/lib/python3.12/site-packages (from marshmallow<4.0.0,>=3.18.0->dataclasses-json->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (26.0)
    Requirement already satisfied: httplib2<1.0.0,>=0.19.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-api-python-client->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (0.32.0)
    Requirement already satisfied: google-auth-httplib2<1.0.0,>=0.2.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-api-python-client->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (0.4.0)
    Requirement already satisfied: uritemplate<5,>=3.0.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-api-python-client->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (4.2.0)
    Requirement already satisfied: pyparsing<4,>=3.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from httplib2<1.0.0,>=0.19.0->google-api-python-client->google-generativeai<0.6.0,>=0.5.2->llama-index-llms-gemini==0.1.11) (3.3.2)
    Requirement already satisfied: python-dateutil>=2.8.2 in /home/codespace/.local/lib/python3.12/site-packages (from pandas->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (2.9.0.post0)
    Requirement already satisfied: six>=1.5 in /home/codespace/.local/lib/python3.12/site-packages (from python-dateutil>=2.8.2->pandas->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-llms-gemini==0.1.11) (1.17.0)
    
    [1m[[0m[34;49mnotice[0m[1;39;49m][0m[39;49m A new release of pip is available: [0m[31;49m26.0.1[0m[39;49m -> [0m[32;49m26.1.2[0m
    [1m[[0m[34;49mnotice[0m[1;39;49m][0m[39;49m To update, run: [0m[32;49mpython3 -m pip install --upgrade pip[0m
    Requirement already satisfied: llama-index-embeddings-gemini==0.1.8 in /usr/local/python/3.12.1/lib/python3.12/site-packages (0.1.8)
    Requirement already satisfied: google-generativeai<0.6.0,>=0.5.2 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-embeddings-gemini==0.1.8) (0.5.4)
    Requirement already satisfied: llama-index-core<0.11.0,>=0.10.11.post1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-embeddings-gemini==0.1.8) (0.10.59)
    Requirement already satisfied: google-ai-generativelanguage==0.6.4 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (0.6.4)
    Requirement already satisfied: google-api-core in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (2.30.3)
    Requirement already satisfied: google-api-python-client in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (2.198.0)
    Requirement already satisfied: google-auth>=2.15.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (2.55.1)
    Requirement already satisfied: protobuf in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (4.25.9)
    Requirement already satisfied: pydantic in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (2.13.4)
    Requirement already satisfied: tqdm in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (4.68.3)
    Requirement already satisfied: typing-extensions in /home/codespace/.local/lib/python3.12/site-packages (from google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (4.15.0)
    Requirement already satisfied: proto-plus<2.0.0dev,>=1.22.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-ai-generativelanguage==0.6.4->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (1.28.0)
    Requirement already satisfied: googleapis-common-protos<2.0.0,>=1.63.2 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-api-core->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (1.75.0)
    Requirement already satisfied: requests<3.0.0,>=2.20.0 in /home/codespace/.local/lib/python3.12/site-packages (from google-api-core->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (2.32.5)
    Requirement already satisfied: grpcio<2.0.0,>=1.33.2 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-api-core[grpc]!=2.0.*,!=2.1.*,!=2.10.*,!=2.2.*,!=2.3.*,!=2.4.*,!=2.5.*,!=2.6.*,!=2.7.*,!=2.8.*,!=2.9.*,<3.0.0dev,>=1.34.1->google-ai-generativelanguage==0.6.4->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (1.81.1)
    Requirement already satisfied: grpcio-status<2.0.0,>=1.33.2 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-api-core[grpc]!=2.0.*,!=2.1.*,!=2.10.*,!=2.2.*,!=2.3.*,!=2.4.*,!=2.5.*,!=2.6.*,!=2.7.*,!=2.8.*,!=2.9.*,<3.0.0dev,>=1.34.1->google-ai-generativelanguage==0.6.4->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (1.62.3)
    Requirement already satisfied: pyasn1-modules>=0.2.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-auth>=2.15.0->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (0.4.2)
    Requirement already satisfied: cryptography>=38.0.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-auth>=2.15.0->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (49.0.0)
    Requirement already satisfied: PyYAML>=6.0.1 in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (6.0.3)
    Requirement already satisfied: SQLAlchemy>=1.4.49 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from SQLAlchemy[asyncio]>=1.4.49->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (2.0.51)
    Requirement already satisfied: aiohttp<4.0.0,>=3.8.6 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (3.14.1)
    Requirement already satisfied: dataclasses-json in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (0.6.7)
    Requirement already satisfied: deprecated>=1.2.9.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (1.3.1)
    Requirement already satisfied: dirtyjson<2.0.0,>=1.0.8 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (1.0.8)
    Requirement already satisfied: fsspec>=2023.5.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (2026.6.0)
    Requirement already satisfied: httpx in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (0.28.1)
    Requirement already satisfied: nest-asyncio<2.0.0,>=1.5.8 in /home/codespace/.local/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (1.6.0)
    Requirement already satisfied: networkx>=3.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (3.6.1)
    Requirement already satisfied: nltk<4.0.0,>=3.8.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (3.9.4)
    Requirement already satisfied: numpy<2.0.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (1.26.4)
    Requirement already satisfied: openai>=1.1.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (1.109.1)
    Requirement already satisfied: pandas in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (3.0.4)
    Requirement already satisfied: pillow>=9.0.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (10.4.0)
    Requirement already satisfied: tenacity!=8.4.0,<9.0.0,>=8.2.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (8.5.0)
    Requirement already satisfied: tiktoken>=0.3.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (0.13.0)
    Requirement already satisfied: typing-inspect>=0.8.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (0.9.0)
    Requirement already satisfied: wrapt in /usr/local/python/3.12.1/lib/python3.12/site-packages (from llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (2.2.2)
    Requirement already satisfied: aiohappyeyeballs>=2.5.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (2.6.2)
    Requirement already satisfied: aiosignal>=1.4.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (1.4.0)
    Requirement already satisfied: attrs>=17.3.0 in /home/codespace/.local/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (25.4.0)
    Requirement already satisfied: frozenlist>=1.1.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (1.8.0)
    Requirement already satisfied: multidict<7.0,>=4.5 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (6.7.1)
    Requirement already satisfied: propcache>=0.2.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (0.5.2)
    Requirement already satisfied: yarl<2.0,>=1.17.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from aiohttp<4.0.0,>=3.8.6->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (1.24.2)
    Requirement already satisfied: click in /usr/local/python/3.12.1/lib/python3.12/site-packages (from nltk<4.0.0,>=3.8.1->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (8.4.2)
    Requirement already satisfied: joblib in /usr/local/python/3.12.1/lib/python3.12/site-packages (from nltk<4.0.0,>=3.8.1->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (1.5.3)
    Requirement already satisfied: regex>=2021.8.3 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from nltk<4.0.0,>=3.8.1->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (2026.6.28)
    Requirement already satisfied: charset_normalizer<4,>=2 in /home/codespace/.local/lib/python3.12/site-packages (from requests<3.0.0,>=2.20.0->google-api-core->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (3.4.5)
    Requirement already satisfied: idna<4,>=2.5 in /home/codespace/.local/lib/python3.12/site-packages (from requests<3.0.0,>=2.20.0->google-api-core->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (3.11)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /home/codespace/.local/lib/python3.12/site-packages (from requests<3.0.0,>=2.20.0->google-api-core->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (2.6.3)
    Requirement already satisfied: certifi>=2017.4.17 in /home/codespace/.local/lib/python3.12/site-packages (from requests<3.0.0,>=2.20.0->google-api-core->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (2026.2.25)
    Requirement already satisfied: cffi>=2.0.0 in /home/codespace/.local/lib/python3.12/site-packages (from cryptography>=38.0.3->google-auth>=2.15.0->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (2.0.0)
    Requirement already satisfied: pycparser in /home/codespace/.local/lib/python3.12/site-packages (from cffi>=2.0.0->cryptography>=38.0.3->google-auth>=2.15.0->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (3.0)
    Requirement already satisfied: anyio<5,>=3.5.0 in /home/codespace/.local/lib/python3.12/site-packages (from openai>=1.1.0->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (4.12.1)
    Requirement already satisfied: distro<2,>=1.7.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from openai>=1.1.0->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (1.9.0)
    Requirement already satisfied: jiter<1,>=0.4.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from openai>=1.1.0->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (0.15.0)
    Requirement already satisfied: sniffio in /usr/local/python/3.12.1/lib/python3.12/site-packages (from openai>=1.1.0->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (1.3.1)
    Requirement already satisfied: httpcore==1.* in /home/codespace/.local/lib/python3.12/site-packages (from httpx->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (1.0.9)
    Requirement already satisfied: h11>=0.16 in /home/codespace/.local/lib/python3.12/site-packages (from httpcore==1.*->httpx->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (0.16.0)
    Requirement already satisfied: annotated-types>=0.6.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from pydantic->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (0.7.0)
    Requirement already satisfied: pydantic-core==2.46.4 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from pydantic->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (2.46.4)
    Requirement already satisfied: typing-inspection>=0.4.2 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from pydantic->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (0.4.2)
    Requirement already satisfied: pyasn1<0.7.0,>=0.6.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from pyasn1-modules>=0.2.1->google-auth>=2.15.0->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (0.6.3)
    Requirement already satisfied: greenlet>=1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from SQLAlchemy>=1.4.49->SQLAlchemy[asyncio]>=1.4.49->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (3.5.3)
    Requirement already satisfied: mypy-extensions>=0.3.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from typing-inspect>=0.8.0->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (1.1.0)
    Requirement already satisfied: marshmallow<4.0.0,>=3.18.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from dataclasses-json->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (3.26.2)
    Requirement already satisfied: packaging>=17.0 in /home/codespace/.local/lib/python3.12/site-packages (from marshmallow<4.0.0,>=3.18.0->dataclasses-json->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (26.0)
    Requirement already satisfied: httplib2<1.0.0,>=0.19.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-api-python-client->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (0.32.0)
    Requirement already satisfied: google-auth-httplib2<1.0.0,>=0.2.0 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-api-python-client->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (0.4.0)
    Requirement already satisfied: uritemplate<5,>=3.0.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from google-api-python-client->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (4.2.0)
    Requirement already satisfied: pyparsing<4,>=3.1 in /usr/local/python/3.12.1/lib/python3.12/site-packages (from httplib2<1.0.0,>=0.19.0->google-api-python-client->google-generativeai<0.6.0,>=0.5.2->llama-index-embeddings-gemini==0.1.8) (3.3.2)
    Requirement already satisfied: python-dateutil>=2.8.2 in /home/codespace/.local/lib/python3.12/site-packages (from pandas->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (2.9.0.post0)
    Requirement already satisfied: six>=1.5 in /home/codespace/.local/lib/python3.12/site-packages (from python-dateutil>=2.8.2->pandas->llama-index-core<0.11.0,>=0.10.11.post1->llama-index-embeddings-gemini==0.1.8) (1.17.0)
    
    [1m[[0m[34;49mnotice[0m[1;39;49m][0m[39;49m A new release of pip is available: [0m[31;49m26.0.1[0m[39;49m -> [0m[32;49m26.1.2[0m
    [1m[[0m[34;49mnotice[0m[1;39;49m][0m[39;49m To update, run: [0m[32;49mpython3 -m pip install --upgrade pip[0m



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

gemini_model = os.getenv("GEMINI_MODEL", "models/gemini-2.5-flash")
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
from typing import List
from llama_index.core import SimpleDirectoryReader
from llama_index.core.node_parser import SentenceSplitter
from llama_index.core import  VectorStoreIndex
from llama_index.core.tools import QueryEngineTool

#-------------------------------------------------------------
# Tool 1 : Function that returns the list of items in an order
#-------------------------------------------------------------
def get_order_items(order_id: int) -> List[str] :
    """Given an order Id, this function returns the 
    list of items purchased for that order"""
    
    order_items = {
            1001: ["Laptop","Mouse"],
            1002: ["Keyboard","HDMI Cable"],
            1003: ["Laptop","Keyboard"]
        }
    if order_id in order_items.keys():
        return order_items[order_id]
    else:
        return []

#-------------------------------------------------------------
# Tool 2 : Function that returns the delivery date for an order
#-------------------------------------------------------------
def get_delivery_date(order_id: int) -> str:
    """Given an order Id, this function returns the 
    delivery date for that order"""

    delivery_dates = {
            1001: "10-Jun",
            1002: "12-Jun",
            1003: "08-Jun"       
    }
    if order_id in delivery_dates.keys():
        return delivery_dates[order_id]
    else:
        return []

#----------------------------------------------------------------
# Tool 3 : Function that returns maximum return days for an item
#----------------------------------------------------------------
def get_item_return_days(item: str) -> int :
    """Given an Item, this function returns the return support
    for that order. The return support is in number of days"""
    
    item_returns = {
            "Laptop"     : 30,
            "Mouse"      : 15,
            "Keyboard"   : 15,
            "HDMI Cable" : 5
    }
    if item in item_returns.keys():
        return item_returns[item]
    else:
        #Default
        return 45

#-------------------------------------------------------------
# Tool 4 : Vector DB that contains customer support contacts
#-------------------------------------------------------------
#Setup vector index for return policies
support_docs=SimpleDirectoryReader(input_files=["Customer Service.pdf"]).load_data()

splitter=SentenceSplitter(chunk_size=1024)
support_nodes=splitter.get_nodes_from_documents(support_docs)
support_index=VectorStoreIndex(support_nodes)
support_query_engine = support_index.as_query_engine()

```

### 06.04. Setup the Customer Service AI Agent


```python
from llama_index.core.tools import FunctionTool

#Create tools for the 3 functions and 1 index
order_item_tool = FunctionTool.from_defaults(fn=get_order_items)
delivery_date_tool = FunctionTool.from_defaults(fn=get_delivery_date)
return_policy_tool = FunctionTool.from_defaults(fn=get_item_return_days)

support_tool = QueryEngineTool.from_defaults(
    query_engine=support_query_engine,
    description=(
        "Customer support policies and contact information"
    ),
)
```


```python
from llama_index.core.agent import ReActAgent

#Setup the Agent in LlamaIndex with all the Tools.
#Gemini is used through a ReAct agent because this LlamaIndex Gemini wrapper
#does not expose the native function-calling interface required by FunctionCallingAgentWorker.
agent = ReActAgent.from_tools(
    [order_item_tool,
     delivery_date_tool,
     return_policy_tool,
     support_tool
    ],
    llm=Settings.llm,
    verbose=True,
    max_iterations=10,
)

```

### 06.05. Using the customer service Agent


```python
#Get return policy for an order
response = agent.query(
    "What is the return policy for order number 1001"
)

print("\n Final output : \n", response)
```

    > Running step c18753aa-236e-448a-a6ac-904aef12ad9f. Step input: What is the return policy for order number 1001
    [1;3;38;5;200mThought: The current language of the user is: English. I need to use a tool to help me answer the question.
    Action: get_order_items
    Action Input: {'order_id': 1001}
    [0m[1;3;34mObservation: ['Laptop', 'Mouse']
    [0m> Running step e5fb5bf8-6b19-43bf-adce-1909f72acdf0. Step input: None
    [1;3;38;5;200mThought: The current language of the user is: English. I have the items for order 1001. Now I need to find the return policy for each item. I will start with 'Laptop'.
    Action: get_item_return_days
    Action Input: {'item': 'Laptop'}
    [0m[1;3;34mObservation: 30
    [0m> Running step 7993fee5-51e1-430c-ae7b-3ab2f6ab77dc. Step input: None
    [1;3;38;5;200mThought: The current language of the user is: English. I have the return policy for 'Laptop'. Now I need to find the return policy for 'Mouse'.
    Action: get_item_return_days
    Action Input: {'item': 'Mouse'}
    [0m[1;3;34mObservation: 15
    [0m> Running step 5d3f40d5-7bbc-4b21-9527-80cb63f404b2. Step input: None
    [1;3;38;5;200mThought: The current language of the user is: English. I have the return policies for all items in order 1001. I also need the delivery date to calculate the return window.
    Action: get_delivery_date
    Action Input: {'order_id': 1001}
    [0m[1;3;34mObservation: 10-Jun
    [0m> Running step cc6f2b20-d7ec-48a9-bf9a-9915d2590813. Step input: None
    [1;3;38;5;200mThought: I can answer without using any more tools. I'll use the user's language to answer
    Answer: The return policy for order number 1001 is as follows:
    - Laptop: 30 days from the delivery date (10-Jun).
    - Mouse: 15 days from the delivery date (10-Jun).
    [0m
     Final output : 
     The return policy for order number 1001 is as follows:
    - Laptop: 30 days from the delivery date (10-Jun).
    - Mouse: 15 days from the delivery date (10-Jun).



```python
# Three part question
response = agent.query(
    "When is the delivery date and items shipped for order 1003 and how can I contact customer support?"
)

print("\n Final output : \n", response)
```

    > Running step f52f86a5-dddb-4995-a8d3-814fe28af7c3. Step input: When is the delivery date and items shipped for order 1003 and how can I contact customer support?
    [1;3;38;5;200mThought: The current language of the user is: English. I need to use a tool to help me answer the question.
    Action: get_delivery_date
    Action Input: {'order_id': 1003}
    [0m[1;3;34mObservation: 08-Jun
    [0m> Running step aa43ef47-87d4-4a42-90bf-fbcd04a07545. Step input: None
    [1;3;38;5;200mThought: The current language of the user is: English. I need to use a tool to help me answer the question.
    Action: get_order_items
    Action Input: {'order_id': 1003}
    [0m[1;3;34mObservation: ['Laptop', 'Keyboard']
    [0m> Running step 4d77bd3d-ac28-46c4-a750-72adf700d691. Step input: None
    [1;3;38;5;200mThought: The current language of the user is: English. I need to use a tool to help me answer the question.
    Action: query_engine_tool
    Action Input: {'input': 'how can I contact customer support?'}
    [0m[1;3;34mObservation: You can contact customer support by calling 1-987-654-3210 or by sending an email to support@company.com.
    [0m> Running step 99d8523a-a8b4-4e00-b519-1710c85dd280. Step input: None
    [1;3;38;5;200mThought: I can answer without using any more tools. I'll use the user's language to answer
    Answer: The delivery date for order 1003 is 08-Jun and the items shipped are ['Laptop', 'Keyboard']. You can contact customer support by calling 1-987-654-3210 or by sending an email to support@company.com.
    [0m
     Final output : 
     The delivery date for order 1003 is 08-Jun and the items shipped are ['Laptop', 'Keyboard']. You can contact customer support by calling 1-987-654-3210 or by sending an email to support@company.com.



```python
#Question about an invalid order number
response = agent.query(
    "What is the return policy for order number 1004"
)

print("\n Final output : \n", response)
```

    > Running step 537c843d-94dd-4a61-bee1-024b2f5a725b. Step input: What is the return policy for order number 1004
    [1;3;38;5;200mThought: The current language of the user is: English. I need to use a tool to help me answer the question.
    Action: get_order_items
    Action Input: {'order_id': 1004}
    [0m[1;3;34mObservation: []
    [0m> Running step 0bf48927-8030-44b1-8f12-cbef46d55928. Step input: None
    [1;3;38;5;200mThought: The current language of the user is: English. The previous tool call returned an empty list of items for order 1004. This means there are no items associated with that order, or the order ID is invalid. I cannot determine the return policy without knowing the items. I should inform the user that the order ID might be invalid or has no items.
    Answer: I could not find any items for order number 1004. Please check if the order number is correct.
    [0m
     Final output : 
     I could not find any items for order number 1004. Please check if the order number is correct.



```python

```
