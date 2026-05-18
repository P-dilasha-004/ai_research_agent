# AI Research Assistant

This project is a multi-agent AI research system that automatically searches the web, collects information, and generates research reports.

Instead of using a single AI prompt, the system divides the work between multiple specialized agents. Each agent handles a specific task, such as web searching, reading webpages, writing the report, and reviewing the final result.

The project also includes an automated Quality Gate that reviews, fact-checks, and scores the generated report to improve reliability and reduce AI hallucinations.

---

# Overview

Given a research topic, the system functions like an autonomous research team:

1. Searches the web for relevant and up-to-date information
2. Scrapes and processes webpage content
3. Generates a research draft based on collected data
4. Reviews and critiques the generated draft
5. Produces a final report along with a quality score and evaluation

The final output includes:
- A thoroughly researched AI-generated report
- An automated peer review from the Critic Chain
- A quality score evaluating the generated response

---

# Workflow Architecture

```text
       [ User Research Topic ]
                 │
                 ▼
        ┌──────────────────┐
        │   Search Agent   │
        │   (Tavily API)   │
        └──────────────────┘
                 │
                 │ state['search_results']
                 ▼
        ┌──────────────────┐
        │   Reader Agent   │
        │ (BeautifulSoup)  │
        └──────────────────┘
                 │
                 │ state['scraped_content']
                 ▼
        ┌──────────────────┐
        │   Writer Chain   │
        └──────────────────┘
                 │
                 │ Initial Draft
                 ▼
        ┌──────────────────┐
        │   Critic Chain   │
        └──────────────────┘
                 │
                 │ Draft + Review Score
                 ▼
          [ Final Output ]
```

---

# System Components

## 1. Search Agent

The Search Agent is powered by an `AgentExecutor` connected to a `web_search_tool`.

### Responsibilities
- Queries the Tavily API
- Retrieves live web search results
- Collects relevant URLs and metadata
- Stores search results in shared workflow state

### State Output

```python
state["search_results"]
```

---

## 2. Reader Agent

The Reader Agent processes the URLs returned by the Search Agent.

### Responsibilities
- Scrapes webpage content
- Extracts meaningful text using BeautifulSoup
- Cleans and structures raw webpage data
- Provides deeper research context

### State Output

```python
state["scraped_content"]
```

---

## 3. Writer Chain

The Writer Chain synthesizes the scraped information into a structured research draft.

### Responsibilities
- Summarizes findings from multiple sources
- Connects related information across webpages
- Generates a detailed research response
- Produces the initial draft

---

## 4. Critic Chain

The Critic Chain acts as an automated quality assurance layer for the generated research.

### Responsibilities
- Reviews the generated draft
- Evaluates clarity and reasoning
- Checks factual consistency
- Detects missing information
- Identifies possible hallucinations
- Produces a quality score and critical review

This creates a feedback and evaluation system that improves the reliability of the final output.

# Technologies Used

| Component | Technology |
|---|---|
| Agent Framework | LangChain / LangGraph |
| Agent Execution | AgentExecutor |
| Search API | Tavily API |
| Web Scraping | BeautifulSoup (`bs4`) |
| Language | Python |
| State Management | Dictionary-based workflow state |

---

# Getting Started

## Prerequisites

You will need API keys for the tools and LLMs used in this project:

- Tavily API Key (for the Search Agent)
- LLM API Key (OpenAI, Anthropic, or another LLM of your choice)

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/P-dilasha-004/ai_research_agent.git
cd ai-research-agent
```

---

### 2. Install Dependencies

Make sure Python is installed, then run:

```bash
pip install -r requirements.txt
```

---

### 3. Configure Environment Variables

Create a `.env` file in the root directory and add your API keys:

```env
TAVILY_API_KEY="your_tavily_key_here"
LLM_API_KEY="your_LLM_key_here"
```

---

# Usage

To run the workflow, execute the main script:

```bash
python pipeline.py
```

You will then be prompted to enter a research topic.

---

# Learning Purpose

This project was built as a learning-focused implementation to better understand how multi-agent AI systems work in practice.
