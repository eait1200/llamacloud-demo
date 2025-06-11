# LlamaIndex Cloud Documentation

This repository contains the LlamaIndex Cloud documentation crawled and converted to Markdown format for easy consumption and knowledge base creation.

## Contents

- `docs/` - Main documentation pages
- `api/` - API reference documentation

## About

This documentation was crawled from https://docs.cloud.llamaindex.ai/ using Firecrawl and organized for easy access and processing.

## LlamaCloud Overview

LlamaCloud is a hosted service for document processing and search, powered by LlamaIndex. It consists of three primary components:

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

## Usage

Get started with [Web UI](https://cloud.llamaindex.ai/), [Python SDK](https://github.com/run-llama/llama_cloud_services), and [REST API](https://docs.cloud.llamaindex.ai/API/llama-platform). [Sign up for an account](https://cloud.llamaindex.ai/login) to get started or explore the documentation for each component.

---

*Last updated: June 2025*
*Source: https://docs.cloud.llamaindex.ai/*

# LlamaCloud Demo

This repository contains a collection of cookbooks to show you how to build LLM applications using LlamaCloud to help manage your data pipelines, and LlamaIndex as the core orchestration framework.

## Getting Started

1. Follow the instructions in the section below for setting up the Jupyter Environment.
1. Go to [https://cloud.llamaindex.ai/](https://cloud.llamaindex.ai/) and create an account using one of the authentication providers.
1. Once logged in, go to [the API Key page](https://cloud.llamaindex.ai/api-key) and create an API key. Copy that generated API key to your clipboard.
1. Go back to LlamaCloud. Create a project and initialize a new index by specifying the data source, data sink, embedding, and optionally transformation parameters. 
1. Open one of the Jupyter notebooks in this repo (e.g. `examples/getting_started.ipynb`) and paste the API key into the first cell block that reads `os.environ["PLATFORM_API_KEY"] = "..."`
1. Copy the `index_name` and `project_name` from the deployed index into the `LlamaCloudIndex` initialization in the notebook.

That should get you started! You should now be able to create an e2e pipeline with a LlamaCloud pipeline as the backend.

## Setting up the Jupyter Environment
Here's some commands for installing the Python dependencies & running Jupyter.
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Notebooks are in `examples`.

Note: if you encounter package issues when running notebook examples, please `rm -rf .venv` and repeat the above steps again.
