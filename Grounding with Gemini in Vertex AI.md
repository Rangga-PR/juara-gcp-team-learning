## Overview

Grounding in Vertex AI lets you use generative text models to generate content grounded in your own documents and data. This capability lets the model access information at runtime that goes beyond its training data. By grounding model responses in Google Search results or data stores within Vertex AI Search, LLMs that are grounded in data can produce more accurate, up-to-date, and relevant responses.

Grounding provides the following benefits:

1. Reduces model hallucinations (instances where the model generates content that isn't factual)
2. Anchors model responses to specific information, documents, and data sources
3. Enhances the trustworthiness, accuracy, and applicability of the generated content

### Setup Google Cloud Project and Initialize Vertext AI SDK

```python
PROJECT_ID = "your-project-id"
REGION = "your-project-region"


import vertexai

vertexai.init(project=PROJECT_ID, location=REGION)
```

### Import Libraries and Initialize Model

```python
from IPython.display import Markdown, display
from vertexai.generative_models import (
    GenerationResponse,
    GenerativeModel,
    Tool,
    grounding,
)
from vertexai.preview.generative_models import grounding as preview_grounding

model = GenerativeModel("gemini-1.5-pro")
```

### Add Helper Functions

```python
def print_grounding_response(response: GenerationResponse):
    """Prints Gemini response with grounding citations."""
    grounding_metadata = response.candidates[0].grounding_metadata

    # Citation indices are in byte units
    ENCODING = "utf-8"
    text_bytes = response.text.encode(ENCODING)

    prev_index = 0
    markdown_text = ""

    for grounding_support in grounding_metadata.grounding_supports:
        text_segment = text_bytes[
            prev_index : grounding_support.segment.end_index
        ].decode(ENCODING)

        footnotes_text = ""
        for grounding_chunk_index in grounding_support.grounding_chunk_indices:
            footnotes_text += f"[{grounding_chunk_index + 1}]"

        markdown_text += f"{text_segment} {footnotes_text}\n"
        prev_index = grounding_support.segment.end_index

    if prev_index < len(text_bytes):
        markdown_text += str(text_bytes[prev_index:], encoding=ENCODING)

    markdown_text += "\n----\n## Grounding Sources\n"

    if grounding_metadata.web_search_queries:
        markdown_text += (
            f"\n**Web Search Queries:** {grounding_metadata.web_search_queries}\n"
        )
        if grounding_metadata.search_entry_point:
            markdown_text += f"\n**Search Entry Point:**\n {grounding_metadata.search_entry_point.rendered_content}\n"
    elif grounding_metadata.retrieval_queries:
        markdown_text += (
            f"\n**Retrieval Queries:** {grounding_metadata.retrieval_queries}\n"
        )

    markdown_text += "### Grounding Chunks\n"

    for index, grounding_chunk in enumerate(
        grounding_metadata.grounding_chunks, start=1
    ):
        context = grounding_chunk.web or grounding_chunk.retrieved_context
        if not context:
            print(f"Skipping Grounding Chunk {grounding_chunk}")
            continue

        markdown_text += f"{index}. [{context.title}]({context.uri})\n"

    display(Markdown(markdown_text))
```

### Grounding with Google Search results

instruct the LLM to first perform a Google Search with the prompt, then construct an answer based on the web search results.

```python
tool = Tool.from_google_search_retrieval(grounding.GoogleSearchRetrieval())

response = model.generate_content(PROMPT, tools=[tool])

print_grounding_response(response)
```

### Grounding with custom documents and data

instruct the LLM to constuct an answer based on the result of a data store in Vertext AI search

```python
DATA_STORE_PROJECT_ID = "your-project-id"
DATA_STORE_REGION = "your-data-store-region"
DATA_STORE_ID = "your-data-store-id"

PROMPT = "When should I use an object table in BigQuery? And how does it store data?"


tool = Tool.from_retrieval(
    preview_grounding.Retrieval(
        preview_grounding.VertexAISearch(
            datastore=DATA_STORE_ID,
            project=DATA_STORE_PROJECT_ID,
            location=DATA_STORE_REGION,
        )
    )
)

response = model.generate_content(PROMPT, tools=[tool])
print_grounding_response(response)
```
