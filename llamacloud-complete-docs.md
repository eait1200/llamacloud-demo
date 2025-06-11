# LlamaIndex Cloud Documentation - Complete Reference

*Crawled from https://docs.cloud.llamaindex.ai/ on June 2025*
*Updated with comprehensive content from all documentation sections*

---

## Table of Contents

1. [Welcome to LlamaCloud](#welcome-to-llamacloud)
2. [Getting Started](#getting-started)
3. [Quick Start Guide](#quick-start-guide)
4. [Retrieval](#retrieval)
   - [Retrieval Modes](#retrieval-modes)
   - [Basic Retrieval](#basic-retrieval)
   - [Advanced Retrieval](#advanced-retrieval)
   - [Composite Retrieval](#composite-retrieval)
5. [Data Sources](#data-sources)
6. [Data Sinks](#data-sinks)
7. [Parsing and Transformation](#parsing-and-transformation)
8. [Embedding Models](#embedding-models)
9. [Guides](#guides)
10. [API Reference](#api-reference)

---

## Welcome to LlamaCloud

LlamaCloud is generally available! You can [sign up for a free account](https://cloud.llamaindex.ai/).

### Overview

LlamaCloud makes it easy to set up a highly scalable & customizable data ingestion pipeline for your RAG use case. No need to worry about scaling challenges, document management, or complex file parsing.

LlamaCloud offers all of this through a no-code UI, REST API / clients, and seamless integration with our popular python & typescript framework.

Connect your index on LlamaCloud to your [data sources](https://docs.cloud.llamaindex.ai/llamacloud/data_sources), set your parse parameters & embedding model, and LlamaCloud automatically handles syncing your data into your [vector databases](https://docs.cloud.llamaindex.ai/llamacloud/data_sinks). From there, we offer an easy-to-use interface to query your indexes and retrieve relevant ground truth information from your input documents.

LlamaCloud consists of three primary components:

### Parse

Parse transforms complex documents into LLM-ready structured data with:

- Support for 50+ file formats (PDF, DOCX, PPTX, XLSX, HTML, EPUB, images)
- Advanced parsing capabilities including tables, charts, and layout extraction
- Multimodal parsing options using vendor models for complex documents
- Customizable parsing with Fast, Balanced, Premium, and Custom modes

### Extract

Extract transforms complex documents into well-typed structured data with:

- Customizable extraction agents and schemas
- Batch processing capabilities for scale
- Iterative schema development

### Index

Index transforms document collections into searchable knowledge bases with:

- Seamless integration with popular vector databases
- Automated syncing from data sources to vector stores
- Built-in query interface for retrieving relevant information
- Customizable indexing pipeline for RAG applications

Get started with [Web UI](https://cloud.llamaindex.ai/), [Python SDK](https://github.com/run-llama/llama_cloud_services), and [REST API](https://docs.cloud.llamaindex.ai/API/llama-platform). [Sign up for an account](https://cloud.llamaindex.ai/login) to get started or explore the documentation for each component.

### Enterprise Customers

If you are an Enterprise interested in using LlamaCloud within a network restricted environment, please [get in touch with us!](https://docs.google.com/forms/d/e/1FAIpQLSdehUJJB4NIYfrPIKoFdF4j8kyfnLhMSH_qYJI_WGQbDWD25A/viewform)

---

## Getting Started

Get started with the [quick start guide](https://docs.cloud.llamaindex.ai/llamacloud/getting_started/quick_start) for setting up an index via the UI, and integrate with your RAG/agent application.

---

## Quick Start Guide

In this quick start guide, we show how to build a RAG application with LlamaCloud.
We'll setup an index via the no-code UI, and integrate the retrieval endpoint in a Colab notebook.

### Prerequisites

1. [Sign up for an account](https://cloud.llamaindex.ai/)
2. Prepare an API key for your preferred embedding model service (e.g. OpenAI).

### Sign in

Sign in via [https://cloud.llamaindex.ai/](https://cloud.llamaindex.ai/)

You should see options to sign in via Google, Github, Microsoft, or email.

### Set up an index via UI

Navigate to `Index` feature via the left navbar.

Click the `Create Index` button. You should see a index configuration form.

**Configure data source - file upload**

Click `Select a data source` dropdown and select `Files`

Drag files into file pond or `click to browse`.

[See full list of data sources and specifications](https://docs.cloud.llamaindex.ai/llamacloud/data_sources)

**Configure data sink - managed**

Select `Fully Managed` data sink.

[See full list of data sinks and specifications](https://docs.cloud.llamaindex.ai/llamacloud/data_sinks)

**Configure embedding model - OpenAI**

Select `OpenAI Embedding` and put in your API key.

[See full list of supported embedding models](https://docs.cloud.llamaindex.ai/llamacloud/embedding_models)

**Configure parsing & transformation settings**

Toggle to enable or disable `Llama Parse`.

Select `Auto` mode for best default transformation setting (specify desired chunks size & chunk overlap as necessary.)

`Manual` mode is coming soon, with additional customizability.

[More details about parsing & transformation settings](https://docs.cloud.llamaindex.ai/llamacloud/parsing_transformation).

After configuring the ingestion pipeline, click `Deploy Index` to kick off ingestion.

### (Optional) Observe and manage your index via UI

You should see an index overview with the latest ingestion status.

**(optional) Test retrieval via playground**

Navigate to `Playground` tab to test your retrieval endpoint.

Select between `Fast`, `Accurate`, and `Advanced` retrieval modes.
Input test query and specify retrieval configurations (e.g. base retrieval and top n after re-ranking).

**(optional) Manage connected data sources (or uploaded files)**

Navigate to `Data Sources` tab to manage your connected data sources.

You can upsert, delete, download, and preview uploaded files.

### Integrate your retrieval endpoint into RAG/agent application

After setting up the index, we can now integrate the retrieval endpoint into our RAG/agent application.
Here, we will use a colab notebook as example.

**Obtain LlamaCloud API key**

Navigate to `API Key` page from left sidebar. Click `Generate New Key` button.

Copy the API key to safe location. You will not be able to retrieve this again.
[More detailed walkthrough](https://docs.cloud.llamaindex.ai/llamacloud/getting_started/getting_started/api_key).

**Setup your RAG/agent application - python notebook**

Install latest python framework:

```bash
pip install llama-index
```

[See detail instructions](https://docs.llamaindex.ai/en/stable/getting_started/installation/)

Navigate to `Overview` tab. Click `Copy` button under `Retrieval Endpoint` card

Now you have a minimal RAG application ready to use!

You can find demo colab notebook [here](https://colab.research.google.com/drive/1yu2dFrJDHYDDiiWYcEuZNodJulg1-QZD?usp=sharing).

---

## Retrieval

Data Retrieval is a key step in any RAG application. The most common use case is to retrieve relevant context from your data to help with a question.

Once data has been ingested into LlamaCloud, you can use the Retrieval API to retrieve relevant context from your data.

### Retrieval Modes

There are 4 Retrieval modes to choose from when using LlamaCloud for retrieval:

- `chunks`
- `files_via_metadata`
- `files_via_content`
- `auto_routed`

This mode can be specified via the `retrieval_mode` parameter on the `LlamaCloudIndex.as_retriever` instance method.
The playground UI can also be configured to use each of these retrieval modes.

#### `chunks` mode

**What does it do?**

When using this mode, retrieval queries will be fullfilled by finding semantically similar chunks from the documents that have been ingested into the index.
This is done by embedding the retrieval query, and then searching within the connected data sink for document chunks whose embeddings are a short distance away from this query embedding.

**When to use it?**

When you want to retrieve specific pieces of information from a large dataset, `chunks` mode is ideal. In particular, fact finding queries about specific sections about a document would be well suited for this retrieval mode.

#### `files_via_metadata` mode

**What does it do?**

In `files_via_metadata` mode, the retrieval process focuses on selecting files based on their metadata attributes. This mode leverages a language model to evaluate the relevance of file metadata to the query. The metadata can include file names, resource information, and custom metadata tags. The selected files are then processed to retrieve the relevant documents.

**When to use it?**

Use `files_via_metadata` mode when your query is expected to match specific metadata attributes of files. This is particularly useful for queries that involve identifying files by their names or other metadata tags, such as when you need to retrieve documents based on specific file properties or when the metadata provides a clear indication of relevance to the query.

This retrieval mode is currently limited to indices with 250 files or less. For queries on indices beyond this limit, please use `files_via_content`.

#### `files_via_content` mode

**What does it do?**

The `files_via_content` mode retrieves files by analyzing the content within the files. It ranks document chunks based on their relevance to the query and selects files accordingly. This mode is designed to understand and process the actual content of the files in their entirety, making it suitable for queries that require summarization of the document's text.

**When to use it?**

Choose `files_via_content` mode when your query requires a holistic analysis of the file's content. This mode is ideal for summarization queries where the file that needs to be summarized is not known beforehand.

#### `auto_routed` mode

**What does it do?**

The `auto_routed` mode is a superset of the other retrieval modes, designed to automatically select the most appropriate retrieval strategy based on the query. It leverages a language model to evaluate the query and determine whether to use `chunks`, `files_via_metadata`, or `files_via_content` mode. This dynamic selection process ensures that the retrieval method aligns with the query's requirements, optimizing the retrieval process.

**When to use it?**

Use `auto_routed` mode when the nature of the query is not predetermined, or when you want the system to intelligently choose the best retrieval strategy. This mode is particularly useful in scenarios where queries can vary widely in their requirements, such as a mix of fact-finding, summarization, and metadata-based queries. By automatically routing the query to the most suitable retriever, `auto_routed` mode provides a more flexible retrieval solution.

### Basic Retrieval

Our Retrieval API allows you to retrieve relevant ground truth text chunks that have been ingested into a Index for a given query.
The following snippets show how to run this basic form of retrieval:

**Python Framework**

```python
import os

os.environ[
    "LLAMA_CLOUD_API_KEY"
] = "llx-..."  # can provide API-key in env or in the constructor later on

from llama_index.indices.managed.llama_cloud import LlamaCloudIndex

# connect to existing index
index = LlamaCloudIndex("my_first_index", project_name="Default")

# configure retriever
# alpha=1.0 restricts it to vector search.
retriever = index.as_retriever(
  dense_similarity_top_k=3,
  alpha=1.0,
  enable_reranking=False,
)
nodes = retriever.retrieve("Example query")
```

**TypeScript Framework**

```typescript
import { LlamaCloudIndex } from "llamaindex";

// connect to existing index
const index = new LlamaCloudIndex({
  name: "example-pipeline",
  projectName: "Default",
  apiKey: process.env.LLAMA_CLOUD_API_KEY, // can provide API-key in the constructor or in the env
});

// configure retriever
const retriever = index.asRetriever({
  similarityTopK: 3,
  sparseSimilarityTopK: 3,
  alpha: 0.5,
  enableReranking: true,
  rerankTopN: 3,
});

const nodes = retriever.retrieve({
  query: "Example Query"
});
```

We can build upon this basic form of retrieval by including things like hybrid search, reranking, and metadata filtering to improve the accuracy of the retrieval.

### Advanced Retrieval

LlamaCloud comes with a few advanced retrieval techniques that allow you to improve the accuracy of the retrieval.

#### Hybrid Search

Hybrid search combines the strengths of both vector search and keyword search to improve retrieval accuracy. By leveraging the advantages of both methods, hybrid search can provide more relevant results.

**Note:** Hybrid search is currently only supported by a few vector databases. See [data sinks](https://docs.cloud.llamaindex.ai/llamacloud/data_sinks) for a list of databases that support this feature.

**How Hybrid Search Works**

1. **Vector Search**: This method uses vector embeddings to find documents that are semantically similar to the query. It is particularly useful for capturing the meaning and context of the query, even if the exact keywords are not present in the documents.

2. **Keyword Search**: This method looks for exact matches of the query keywords in the documents. It is effective for finding documents that contain specific terms.

By combining these two methods, hybrid search can return results that are both contextually relevant and contain the specific keywords from the query.

Here's how you can include hybrid search in your Retrieval API requests:

**Python Framework**

```python
import os

os.environ[
    "LLAMA_CLOUD_API_KEY"
] = "llx-..."  # can provide API-key in env or in the constructor later on

from llama_index.indices.managed.llama_cloud import LlamaCloudIndex

# connect to existing index
index = LlamaCloudIndex("my_first_index", project_name="Default")

# configure retriever
retriever = index.as_retriever(
  dense_similarity_top_k=3,
  sparse_similarity_top_k=3,
  alpha=0.5,
  enable_reranking=False,
)
nodes = retriever.retrieve("Example query")
```

#### Re-ranking

Re-ranking is a technique used to improve the order of search results by applying ranking models to the initial set of retrieved document chunks. This can help in presenting the most relevant chunks at the top of the search results.
One common technique is to set a high top-k value, then use re-ranking to improve the order of the results, and then choose the first few results from the re-ranked results as the basis for your final response.

Here's how you can include re-ranking in your Retrieval API requests:

**Python Framework**

```python
import os

os.environ[
    "LLAMA_CLOUD_API_KEY"
] = "llx-..."  # can provide API-key in env or in the constructor later on

from llama_index.indices.managed.llama_cloud import LlamaCloudIndex

# connect to existing index
index = LlamaCloudIndex("my_first_index", project_name="Default")

# configure retriever
retriever = index.as_retriever(
  dense_similarity_top_k=3,
  sparse_similarity_top_k=3,
  alpha=0.5,
  enable_reranking=True,
  rerank_top_n=3,
)
nodes = retriever.retrieve("Example query")
```

**TypeScript Framework**

```typescript
import { LlamaCloudIndex } from "llamaindex";

// connect to existing index
const index = new LlamaCloudIndex({
  name: "example-pipeline",
  projectName: "Default",
  apiKey: process.env.LLAMA_CLOUD_API_KEY, // can provide API-key in the constructor or in the env
});

// configure retriever
const retriever = index.asRetriever({
  similarityTopK: 3,
  sparseSimilarityTopK: 3,
  alpha: 0.5,
  enableReranking: true,
  rerankTopN: 3,
});

const nodes = retriever.retrieve({
  query: "Example Query"
});
```

#### Metadata Filtering

Metadata filtering allows you to narrow down your search results based on specific attributes or tags associated with the documents. This can be particularly useful when you have a large dataset and want to focus on a subset of documents that meet certain criteria.

Here are a few use cases where metadata filtering would be useful:

- Only retrieve chunks from a set of specific files
- Implement access control by filtering by User IDs or User Group IDs that each document is associated with
- Filter documents based on their creation or modification date to retrieve the most recent or relevant information.
- Apply metadata filtering to focus on documents that contain specific tags or categories, such as "financial reports" or "technical documentation."

Here's how you can include metadata filtering in your Retrieval API requests:

**Python Framework**

```python
import os

os.environ[
    "LLAMA_CLOUD_API_KEY"
] = "llx-..."  # can provide API-key in env or in the constructor later on

from llama_index.indices.managed.llama_cloud import LlamaCloudIndex
from llama_index.core.vector_stores import (
    MetadataFilter,
    MetadataFilters,
    FilterOperator,
)

# connect to existing index
index = LlamaCloudIndex("my_first_index", project_name="Default")

# create metadata filter
filters = MetadataFilters(
    filters=[
        MetadataFilter(
            key="theme", operator=FilterOperator.EQ, value="Fiction"
        ),
    ]
)

# configure retriever
retriever = index.as_retriever(
  dense_similarity_top_k=3,
  sparse_similarity_top_k=3,
  alpha=0.5,
  enable_reranking=True,
  rerank_top_n=3,
  filters=filters,
)
nodes = retriever.retrieve("Example query")
```

**TypeScript Framework**

```typescript
import { LlamaCloudIndex } from "llamaindex";

// connect to existing index
const index = new LlamaCloudIndex({
  name: "example-pipeline",
  projectName: "Default",
  apiKey: process.env.LLAMA_CLOUD_API_KEY, // can provide API-key in the constructor or in the env
});

// create metadata filter
const filters = {
  filters: [
    {
      key: "theme",
      operator: "==",
      value: "Fiction",
    },
  ],
};

// configure retriever
const retriever = index.asRetriever({
  similarityTopK: 3,
  sparseSimilarityTopK: 3,
  alpha: 0.5,
  enableReranking: true,
  rerankTopN: 3,
});

const nodes = retriever.retrieve({
  query: "Example Query"
  preFilters: filters
});
```

#### Metadata Filter Inference

Metadata Filter Inference automatically infers search filters from a query using a metadata schema. This can help improve search precision by leveraging metadata attributes without manual filter specification.

This is very similar to Auto-Retrieval from the [LlamaIndex framework](https://docs.llamaindex.ai/en/stable/examples/vector_stores/elasticsearch_auto_retriever/).

Here are a few use cases where metadata filter inference would be useful:

- Automatically filter documents based on inferred criteria from the query.
- Account for temporal context in the query.
  - e.g. a user's query for "What are the latest updates from the past year?" should be filtered to only include documents from the past year.

Using the `search_filters_inference_schema` parameter, you can specify a [Pydantic](https://docs.pydantic.dev/latest/) model that will be used to infer the filters from the query.
You will need to carefully craft the docstring & field descriptions to ensure the model can infer the correct filters.

Here's how you can include metadata filter inference in your Retrieval API requests:

**Python Framework**

```python
import os
from pydantic import BaseModel, Field
from llama_index.indices.managed.llama_cloud import LlamaCloudIndex

# Set your API key
os.environ["LLAMA_CLOUD_API_KEY"] = "llx-..."  # Replace with your actual API key

# Define a Pydantic model for the metadata schema
# The docstring & field description are used to describe the schema to the model.
class MetadataSchema(BaseModel):
    """
    Metadata schema for the index.
    """
    theme: str = Field(description="The theme of the document. Starts with a uppercase letter.")
    author: str = Field(description="The author of the document. First name only, starts with a uppercase letter.")

# Connect to an existing index
index = LlamaCloudIndex("my_first_index", project_name="Default")

# Configure retriever with metadata filter inference
retriever = index.as_retriever(
    dense_similarity_top_k=3,
    sparse_similarity_top_k=3,
    alpha=0.5,
    enable_reranking=True,
    rerank_top_n=3,
    search_filters_inference_schema=MetadataSchema,
)

# Perform retrieval with inferred filters
nodes = retriever.retrieve("Find documents about Fiction by Alice")
# all returned nodes will have metadata where theme="Fiction" and author="Alice"
```

**Crafting the Schema**

The schema descriptions, docstrings, and types need to be described very deliberately as they are provided to the model to infer the filters from the query.

Here are some tips for crafting the schema for metadata filter inference:

- Fill in the docstring on the Pydantic model to describe the schema to the model.
- Use the `description` field to describe the schema to the model.
- Use a granular `type` annotation on the field.
- Specify any casing requirements in the `description` field.
- When describing a date/datetime field, use the `date` or `datetime` type. Descirbe the format of the date/datetime in the `description` field.
- When a field is a string from a constrained set of values, use an `Enum` type.

### Composite Retrieval

#### What is Composite Retrieval?

The Composite Retrieval API allows you to set up a `Retriever` entity that can do retrieval overal several indices at once. This allows you to query across several sources of information at once, further enhancing retrieval relevancy and breadth.

#### When do you need to use this?

A single index can ingest a variety of file or document types. These files can be formatted in various ways _(e.g. a SEC 10K filing may be formatted very differently to a PowerPoint slide show)_.

However, when all these various files/documents are ingested through the same single index, they will be subjected to the same parsing & chunking parameters, regardless of any individual differences in their formatting.
This can be problematic as it can lead to sub-par downstream retrieval performance.

For example, a slideshow containing many images may require multi-modal parsing whereas an financial reports would be more concerned with accurate table and chart extraction. Ideally, your slideshows can be parsed and chunked differently than your financial reports are.
To do so, you should put your slideshow files in an index named "Slide Shows" and your financial reports an an index named "Financial Reports":

```python
import os
from llama_index.indices.managed.llama_cloud import LlamaCloudIndex

# Set your LlamaCloud API Key in LLAMA_CLOUD_API_KEY
assert os.environ.get("LLAMA_CLOUD_API_KEY")
project_name = "My Project"

# Setup your indices
slides_index = LlamaCloudIndex.from_documents(
    documents=[],  # leave documents empty since we will be uploading the raw files
    name="Slides",
    project_name=project_name,
)

# Add your slide files to the index
slides_directory = "./data/slides"
for file_name in os.listdir(slides_directory):
    file_path = os.path.join(slides_directory, file_name)
    # Add each file to the slides index
    slides_index.upload_file(file_path, wait_for_ingestion=False)

# Do the same with your Financial Report files
financial_index = LlamaCloudIndex.from_documents(
    documents=[],  # leave documents empty since we will be uploading the raw files
    name="Financial Reports",
    project_name=project_name,
)

financial_reports_directory = "./data/financial_reports"
for file_name in os.listdir(financial_reports_directory):
    file_path = os.path.join(financial_reports_directory, file_name)
    # Add each file to the slides index
    financial_index.upload_file(file_path, wait_for_ingestion=False)

# wait for both to finish ingestion
slides_index.wait_for_completion()
financial_index.wait_for_completion()
```

Now that you have these files in separate indicies, you can edit the parsing and chunking settings for these datasets independently either via the LlamaCloud UI or via the API.

However, when you want to retrieve data from these, you're still only able to do so one index at a time via `index.as_retriever().retrieve("my query")`.
Your application likely wants to use all of the information you've indexed across both of these indices. You can unify both of these retrievers by creating a _Composite_ Retriever:

```python
from llama_cloud import CompositeRetrievalMode
from llama_index.indices.managed.llama_cloud import LlamaCloudCompositeRetriever

composite_retriever = LlamaCloudCompositeRetriever(
    name="My App Retriever",
    project_name=project_name,
    # If a Retriever named "My App Retriever" doesn't already exist, one will be created
    create_if_not_exists=True,
    # CompositeRetrievalMode.FULL will query each index individually and globally rerank results at the end
    mode=CompositeRetrievalMode.FULL,
    # return the top 5 results from all queried indices
    rerank_top_n=5,
)

# Add the above indices to the composite retriever
# Carefully craft the description as this is used internally to route a query to an attached sub-index when CompositeRetrievalMode.ROUTING is used
composite_retriever.add_index(
    slides_index,
    description="Information source for slide shows presented during team meetings",
)
composite_retriever.add_index(
    financial_index,
    description="Information source for company financial reports",
)

# Start querying across both of these indices at once
nodes = retriever.retrieve("What was the key feature of the highest revenue product in 2024 Q4?")
```

With the above code, you can now query across all of your organizational knowledge, spread across a heterogenous dataset of files, _without_ having to sacrifice retrieval quality.

#### Composite Retrieval Modes

There are currently two Composite Retrieval Modes:

- `full` - In this mode, all attached sub-indices will be queried and reranking will be executed across all nodes received from these sub-indices.
- `routed` - In this mode, an agent determines which sub-indices are most relevant to the provided query _(based on the sub-index's `name` & `description` you've provided)_ and only queries those indices that are deemed relevant. Only the nodes from that chosen subset of indices are then reranked before being returned in the retrieval response.
  - Note: If you plan on using this mode, ensure that the `name` & `description` you give each sub-index in your Retriever is carefully crafted to assist the agent in accurately routing your queries.

---

## Data Sources

A data source is a connection to the data you want to be retrieved as part of your RAG use case.
We support a variety of integrations that automatically connect to your data:

### Supported Data Sources

- **File Upload Data Source** - Guide to uploading files directly to LlamaCloud as a data source, including UI, API, and client instructions.
- **S3 Data Source** - Guide to configuring Amazon S3 as a data source in LlamaCloud, including UI, API, client setup, and required AWS permissions.
- **Azure Blob Storage Data Source** - Guide to configuring Azure Blob Storage as a data source in LlamaCloud, including UI, API, and client setup for multiple authentication methods.
- **Microsoft OneDrive Data Source** - Guide to configuring Microsoft OneDrive as a data source in LlamaCloud, including UI, API, and client setup.
- **Microsoft SharePoint Data Source** - Guide to configuring Microsoft SharePoint as a data source in LlamaCloud, including authentication setup, UI, API, and client instructions.
- **Slack Data Source** - Guide to configuring Slack as a data source in LlamaCloud, including UI, API, client setup, and steps to generate a Slack Bot token.
- **Notion Data Source** - Guide to configuring Notion as a data source in LlamaCloud, including UI, API, client setup, and instructions for finding IDs and integration tokens.
- **Jira Data Source** - Guide to configuring Jira as a data source in LlamaCloud, including UI, API, client setup, and multiple authentication methods.
- **Confluence Data Source** - Guide to configuring Confluence as a data source in LlamaCloud, including UI, API, client setup, and OAuth 2.0 token creation.
- **Box Storage Data Source** - Guide to configuring Box Storage as a data source in LlamaCloud, including UI, API, client setup, and authentication methods.
- **Google Drive Data Source** - Guide to configuring Google Drive as a data source in LlamaCloud, including UI, API, client setup, and service account configuration.

Once the files from your Data Source have been loaded into LlamaCloud, they will be pre-processed before being sent to their final destination in the [Data Sink](https://docs.cloud.llamaindex.ai/llamacloud/data_sinks). This is where you'll setup the parsing configuration for your index.

---

## Data Sinks

Once your input documents have been processed, they're ready to be sent to their final destination: a vector database.

If you don't want to set up and host a vector database, we offer a full-managed option in which we host the vector database for you.
Alternatively, you can host your own vector database and connect it to LlamaCloud:

### Supported Data Sinks

- **Managed Data Sink** - Use LlamaCloud managed index as data sink.
- **Azure AI Search** - Configure via UI
- **Milvus** - Configure your own Milvus Vector DB instance as data sink.
- **MongoDB Atlas Vector Search** - Configure your own MongoDB Atlas instance as data sink.
- **Pinecone** - Configure your own Pinecone instance as data sink.
- **Qdrant** - Configure your own Qdrant instance as data sink.

Once the vector database is setup, they will be store using a [Embedding Model](https://docs.cloud.llamaindex.ai/llamacloud/embedding_models) of choice and will be ready to be used in your RAG use case.

**Note:** For the time being, the term "Data Sink" means a vector database. However, this definition of a Data Sink may expand in the future.

---

## Parsing and Transformation

Once data is loaded from a Data Source, it is pre-processed before being sent to the Data Sink.
There are many pre-processing parameters that can be tweaked to optimize the downstream retrieval performance of your index. While LlamaCloud sets you up with
reasonable defaults, you can dig deeper and customize them as you see fit for your specific use case.

### Parser Settings

A key step of any RAG pipeline is converting your input file into a format that can be used to generate a vector embedding.
There are many parameters that can be used to tweak this conversion process to optimize for your use case.
LlamaCloud sets you up from the start with reasonable defaults for your parsing configurations, but also allows you to dig deeper and customize them as you see fit for your specific application.

### Transformation Settings

The transform configuration is used to define the transformation of the data before it is ingested into the Index. it is a JSON object which you can choose between two modes `auto` and `advanced` and as the name suggests, the `auto` mode is handled by LlamaCloud which uses a set of default configurations and the `advanced` mode is handled by the user with the ability to define their own transformation.

#### Auto Mode

You can set the mode by passing the `transform_config` as below on index creation or update.

```json
{
    "mode": "auto"
}
```

Also when using the `auto` mode, you can configure the chunk size being used for the transformation by passing the `chunk_size` and `chunk_overlap` parameter as below.

```json
{
    "mode": "auto",
    "chunk_size": 1000,
    "chunk_overlap": 100
}
```

#### Advanced Mode

The advanced mode provides a variation of configuration options for the user to define their own transformation. The advanced mode is defined by the `mode` parameter as `advanced` and the `segmentation_config` and `chunking_config` parameters are used to define the segmentation and chunking configuration respectively.

```json
{
    "mode": "advanced",
    "segmentation_config": {
        "mode": "page",
        "page_separator": "\n---\n"
    },
    "chunking_config": {
        "mode": "sentence",
        "separator": " ",
        "paragraph_separator": "\n"
    }
}
```

#### Segmentation Configuration

The segmentation configuration uses the document structure and/or semantics to divide the documents into smaller parts following natural segmentation boundaries. The `segmentation_config` parameter include three modes `none`, `page` and `element`.

**None Segmentation Configuration**

The `none` segmentation configuration is used to define no segmentation.

```json
{
    "mode": "advanced",
    "segmentation_config": {
        "mode": "none"
    }
}
```

**Page Segmentation Configuration**

The `page` segmentation configuration is used to define the segmentation by page and the `page_separator` parameter is used to define the separator, which will split your document into pages.

```json
{
    "mode": "advanced",
    "segmentation_config": {
        "mode": "page",
        "page_separator": "\n---\n"
    }
}
```

**Element Segmentation Configuration**

The `element` segmentation configuration is used to define the segmentation by element which identifies the elements from the document as title, paragraph, list, table, etc.

**Note:** The `element` segmentation configuration is not available with fast parse mode.

```json
{
    "mode": "advanced",
    "segmentation_config": {
        "mode": "element"
    }
}
```

#### Chunking Configuration

Chunking configuration is mainly used to deal with context window limitaitons of embeddings model and LLMs. Conceptually, it's the step after segmenting, where segments are further broken down into smaller chunks as necessary to fit into the context window. It include a few modes `none`, `character`, `token`, `sentence` and `semantic`.

Also all chunk config modes allow the user to define the `chunk_size` and `chunk_overlap` parameters. In the examples below we are not always defining the chunk_size and chunk_overlap parameters but you can always define them.

**None Chunking Configuration**

The `none` chunking configuration is used to define no chunking.

```json
{
    "mode": "advanced",
    "chunking_config": {
        "mode": "none"
    }
}
```

**Character Chunking Configuration**

The `character` chunking configuration is used to define the chunking by character and the `chunk_size` parameter is used to define the size of the chunk.

```json
{
    "mode": "advanced",
    "chunking_config": {
        "mode": "character",
        "chunk_size": 1000
    }
}
```

**Token Chunking Configuration**

The `token` chunking configuration is used to define the chunking by token and uses [OpenAI tokenizer](https://github.com/openai/tiktoken) behind the hood. Also `chunk_size` and `chunk_overlap` parameters are used to define the size of the chunk and the overlap between the chunks.

```json
{
    "mode": "advanced",
    "chunking_config": {
        "mode": "token",
        "chunk_size": 1000,
        "chunk_overlap": 100
    }
}
```

**Sentence Chunking Configuration**

The `sentence` chunking configuration is used to define the chunking by sentence and the `separator` and `paragraph_separator` parameters are used to define the separator between the sentences and paragraphs.

```json
{
    "mode": "advanced",
    "chunking_config": {
        "mode": "sentence",
        "separator": " ",
        "paragraph_separator": "\n"
    }
}
```

### Embedding Model

The embedding model allows you to construct a numerical representation of the text within your files. This is a crucial step in allowing you to search for specific information within your files.
There are a wide variety of embedding models to choose from, and we support quite a few on LlamaCloud.

After Pre-Processing, your data is ready to be sent to the Data Sink.

---

## Embedding Models

Once your input documents have been processed, they will go through a Embedding Model to convert them into vectors.
We support a variety of embedding models that you can choose from:

### Supported Embedding Models

- **OpenAI Embedding** - Embed data using OpenAI's API.
- **Azure Embedding** - Embed data using Azure's API.
- **Cohere Embedding** - Embed data using Cohere's API.
- **Gemini Embedding** - Embed data using Gemini's API.
- **Bedrock Embedding** - Embed data using AWS Bedrock's API.
- **HuggingFace Embedding** - Embed data using HuggingFace's Inference API.

Now that you've set up an Index end-to-end, you're ready to start [retrieving relevant context](https://docs.cloud.llamaindex.ai/llamacloud/retrieval/basic) from your data.

---

## Guides

You can use LlamaCloud through a no-code UI, REST API / clients, and seamless integration with our popular python & typescript framework.

### Available Guides

- **LlamaCloud No-code UI Guide** - Step-by-step guide to using the LlamaCloud no-code UI for creating, managing, and testing indexes and data pipelines.
- **LlamaCloud Framework Integration** - Guide to integrating LlamaCloud with Python and TypeScript frameworks, including index creation, connection, and usage in RAG/agent applications.
- **LlamaCloud API & Clients Guide** - Comprehensive guide to using the LlamaCloud API and SDK clients for Python and TypeScript, including setup, file upload, data source/sink configuration, and index management.

---

## API Reference

### Authentication Endpoints

#### Get Extraction Agent
- **Endpoint**: `GET /api/v1/extraction/agents/{agent_id}`
- **Description**: Retrieves information about a specific extraction agent
- **Authentication**: Required

#### Get User Role
- **Endpoint**: `GET /api/v1/organizations/{org_id}/users/{user_id}/role`
- **Description**: Gets the role of a user within an organization
- **Authentication**: Required

### Embedding Model Configuration

#### Delete Embedding Model Config
- **Endpoint**: `DELETE /api/v1/embedding-models/{config_id}`
- **Description**: Deletes an embedding model configuration
- **Authentication**: Required

### Parsing Jobs

#### Get Job JSON Raw Result
- **Endpoint**: `GET /api/v1/parsing/jobs/{job_id}/result/json/raw`
- **Description**: Retrieves the raw JSON result of a parsing job
- **Authentication**: Required

#### Get Job Raw Markdown Result
- **Endpoint**: `GET /api/v1/parsing/jobs/{job_id}/result/markdown/raw`
- **Description**: Retrieves the raw markdown result of a parsing job
- **Authentication**: Required

#### Get Job Raw Text Result
- **Endpoint**: `GET /api/v1/parsing/jobs/{job_id}/result/text/raw`
- **Description**: Retrieves the raw text result of a parsing job
- **Authentication**: Required

#### Get Job Raw Structured Result
- **Endpoint**: `GET /api/v1/parsing/jobs/{job_id}/result/structured/raw`
- **Description**: Retrieves the raw structured result of a parsing job
- **Authentication**: Required

#### Get Parsing Job Details
- **Endpoint**: `GET /api/v1/parsing/jobs/{job_id}/details`
- **Description**: Gets detailed information about a parsing job
- **Authentication**: Required

#### Get Job Result
- **Endpoint**: `GET /api/v1/parsing/jobs/{job_id}/result`
- **Description**: Retrieves the result of a parsing job
- **Authentication**: Required

#### Get Job Text Result
- **Endpoint**: `GET /api/v1/parsing/jobs/{job_id}/result/text`
- **Description**: Retrieves the text result of a parsing job
- **Authentication**: Required

#### Get Job Image Result
- **Endpoint**: `GET /api/v1/parsing/jobs/{job_id}/result/images`
- **Description**: Retrieves the image results of a parsing job
- **Authentication**: Required

#### Get Parsing History Result
- **Endpoint**: `GET /api/v1/parsing/jobs/{job_id}/history`
- **Description**: Gets the parsing history for a job
- **Authentication**: Required

### File Management

#### Get File Page Screenshot
- **Endpoint**: `GET /api/v1/files/{file_id}/screenshot`
- **Description**: Retrieves a screenshot of a file page
- **Authentication**: Required

#### Get File
- **Endpoint**: `GET /api/v1/files/{file_id}`
- **Description**: Retrieves file information
- **Authentication**: Required

#### Generate Presigned URL
- **Endpoint**: `PUT /api/v1/files/presigned-url`
- **Description**: Generates a presigned URL for file upload
- **Authentication**: Required

### Project Management

#### Get Project Usage
- **Endpoint**: `GET /api/v1/projects/{project_id}/usage`
- **Description**: Retrieves usage statistics for a project
- **Authentication**: Required

#### Get Current Project
- **Endpoint**: `GET /api/v1/projects/current`
- **Description**: Gets information about the current project
- **Authentication**: Required

#### Get Project
- **Endpoint**: `GET /api/v1/projects/{project_id}`
- **Description**: Retrieves project information
- **Authentication**: Required

#### Create Project
- **Endpoint**: `POST /api/v1/projects`
- **Description**: Creates a new project
- **Authentication**: Required

#### Delete Project
- **Endpoint**: `DELETE /api/v1/projects/{project_id}`
- **Description**: Deletes a project
- **Authentication**: Required

### Batch Processing

#### Create Batch
- **Endpoint**: `POST /api/v1/beta/batches`
- **Description**: Creates a new batch processing job
- **Authentication**: Required

#### Get Batch
- **Endpoint**: `GET /api/v1/beta/batches/{batch_id}`
- **Description**: Retrieves information about a batch job
- **Authentication**: Required

### Organization Management

#### Get Organization
- **Endpoint**: `GET /api/v1/organizations/{org_id}`
- **Description**: Retrieves organization information
- **Authentication**: Required

#### Delete Organization
- **Endpoint**: `DELETE /api/v1/organizations/{org_id}`
- **Description**: Deletes an organization
- **Authentication**: Required

#### Get Default Organization
- **Endpoint**: `GET /api/v1/organizations/default`
- **Description**: Gets the default organization
- **Authentication**: Required

#### Get Organization Usage
- **Endpoint**: `GET /api/v1/organizations/{org_id}/usage`
- **Description**: Retrieves organization usage statistics
- **Authentication**: Required

#### Assign Role To User In Organization
- **Endpoint**: `POST /api/v1/organizations/{org_id}/users/{user_id}/role`
- **Description**: Assigns a role to a user in an organization
- **Authentication**: Required

#### Add User To Project
- **Endpoint**: `POST /api/v1/organizations/{org_id}/projects/{project_id}/users/{user_id}`
- **Description**: Adds a user to a project
- **Authentication**: Required

### Report Management

#### Delete Report
- **Endpoint**: `DELETE /api/v1/reports/{report_id}`
- **Description**: Deletes a report
- **Authentication**: Required

#### Get Report Events
- **Endpoint**: `GET /api/v1/reports/{report_id}/events`
- **Description**: Retrieves events for a report
- **Authentication**: Required

#### Get Report
- **Endpoint**: `GET /api/v1/reports/{report_id}`
- **Description**: Retrieves report information
- **Authentication**: Required

#### Create Report
- **Endpoint**: `POST /api/v1/reports`
- **Description**: Creates a new report
- **Authentication**: Required

### Pipeline Management

#### Import Pipeline Metadata
- **Endpoint**: `POST /api/v1/pipelines/{pipeline_id}/metadata/import`
- **Description**: Imports metadata for a pipeline
- **Authentication**: Required

#### Add Files To Pipeline
- **Endpoint**: `POST /api/v1/pipelines/{pipeline_id}/files`
- **Description**: Adds files to a pipeline
- **Authentication**: Required

#### Delete Pipeline File
- **Endpoint**: `DELETE /api/v1/pipelines/{pipeline_id}/files/{file_id}`
- **Description**: Deletes a file from a pipeline
- **Authentication**: Required

#### Get Pipeline Document Status
- **Endpoint**: `GET /api/v1/pipelines/{pipeline_id}/documents/{doc_id}/status`
- **Description**: Gets the status of a document in a pipeline
- **Authentication**: Required

#### Get Pipeline Document
- **Endpoint**: `GET /api/v1/pipelines/{pipeline_id}/documents/{doc_id}`
- **Description**: Retrieves a document from a pipeline
- **Authentication**: Required

#### Create Batch Pipeline Documents
- **Endpoint**: `POST /api/v1/pipelines/{pipeline_id}/documents/batch`
- **Description**: Creates multiple documents in a pipeline
- **Authentication**: Required

#### Create Pipeline
- **Endpoint**: `POST /api/v1/pipelines`
- **Description**: Creates a new pipeline
- **Authentication**: Required

#### Get Pipeline File Status Counts
- **Endpoint**: `GET /api/v1/pipelines/{pipeline_id}/files/status/counts`
- **Description**: Gets file status counts for a pipeline
- **Authentication**: Required

#### Delete Pipeline Document
- **Endpoint**: `DELETE /api/v1/pipelines/{pipeline_id}/documents/{doc_id}`
- **Description**: Deletes a document from a pipeline
- **Authentication**: Required

#### Copy Pipeline
- **Endpoint**: `POST /api/v1/pipelines/{pipeline_id}/copy`
- **Description**: Creates a copy of a pipeline
- **Authentication**: Required

#### Get Playground Session
- **Endpoint**: `GET /api/v1/pipelines/{pipeline_id}/playground/session`
- **Description**: Gets a playground session for a pipeline
- **Authentication**: Required

#### Cancel Pipeline Sync
- **Endpoint**: `POST /api/v1/pipelines/{pipeline_id}/sync/cancel`
- **Description**: Cancels a pipeline synchronization
- **Authentication**: Required

#### Chat
- **Endpoint**: `POST /api/v1/pipelines/{pipeline_id}/chat`
- **Description**: Initiates a chat session with a pipeline
- **Authentication**: Required

#### Get Pipeline Status
- **Endpoint**: `GET /api/v1/pipelines/{pipeline_id}/status`
- **Description**: Gets the current status of a pipeline
- **Authentication**: Required

#### Get Pipeline Data Source Status
- **Endpoint**: `GET /api/v1/pipelines/{pipeline_id}/data-sources/{source_id}/status`
- **Description**: Gets the status of a data source in a pipeline
- **Authentication**: Required

### Retriever Management

#### Create Retriever
- **Endpoint**: `POST /api/v1/retrievers`
- **Description**: Creates a new retriever
- **Authentication**: Required

#### Delete Retriever
- **Endpoint**: `DELETE /api/v1/retrievers/{retriever_id}`
- **Description**: Deletes a retriever
- **Authentication**: Required

#### Direct Retrieve
- **Endpoint**: `POST /api/v1/retrievers/{retriever_id}/retrieve`
- **Description**: Performs a direct retrieval operation
- **Authentication**: Required

### Chat Applications

#### Get Chat App
- **Endpoint**: `GET /api/v1/apps/{app_id}`
- **Description**: Retrieves information about a chat application
- **Authentication**: Required

#### Create Chat App
- **Endpoint**: `POST /api/v1/apps`
- **Description**: Creates a new chat application
- **Authentication**: Required

#### Get Chat Apps
- **Endpoint**: `GET /api/v1/apps`
- **Description**: Lists all chat applications
- **Authentication**: Required

#### Chat With Chat App
- **Endpoint**: `POST /api/v1/apps/{app_id}/chat`
- **Description**: Initiates a chat with a chat application
- **Authentication**: Required

### Extraction Management

#### Get Job
- **Endpoint**: `GET /api/v1/extraction/jobs/{job_id}`
- **Description**: Retrieves information about an extraction job
- **Authentication**: Required

#### Create Extraction Agent
- **Endpoint**: `POST /api/v1/extraction/agents`
- **Description**: Creates a new extraction agent
- **Authentication**: Required

#### Get Extraction Agent By Name
- **Endpoint**: `GET /api/v1/extraction/agents/by-name/{agent_name}`
- **Description**: Retrieves an extraction agent by name
- **Authentication**: Required

#### Delete Extraction Agent
- **Endpoint**: `DELETE /api/v1/extraction/agents/{agent_id}`
- **Description**: Deletes an extraction agent
- **Authentication**: Required

#### Get Run
- **Endpoint**: `GET /api/v1/extraction/runs/{run_id}`
- **Description**: Retrieves information about an extraction run
- **Authentication**: Required

#### Get Run By Job Id
- **Endpoint**: `GET /api/v1/extraction/runs/by-job-id/{job_id}`
- **Description**: Retrieves extraction runs by job ID
- **Authentication**: Required

### Data Management

#### Create Data Sink
- **Endpoint**: `POST /api/v1/data-sinks`
- **Description**: Creates a new data sink
- **Authentication**: Required

#### Create Data Source
- **Endpoint**: `POST /api/v1/data-sources`
- **Description**: Creates a new data source
- **Authentication**: Required

#### Get Data Sink
- **Endpoint**: `GET /api/v1/data-sinks/{sink_id}`
- **Description**: Retrieves information about a data sink
- **Authentication**: Required

### General Job Management

#### Get Jobs
- **Endpoint**: `GET /api/v1/jobs`
- **Description**: Lists all jobs
- **Authentication**: Required

---

*This documentation represents a comprehensive crawl of the LlamaIndex Cloud documentation as of June 2025, including detailed information about retrieval modes, advanced features, and complete API reference. For the most up-to-date information, please visit https://docs.cloud.llamaindex.ai/*