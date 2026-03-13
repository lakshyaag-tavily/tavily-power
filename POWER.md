---
name: "tavily"
displayName: "Tavily Web Search"
description: "Search the web, extract content from URLs, crawl websites, and conduct AI-powered research using Tavily's LLM-optimized APIs"
keywords: ["search", "web-search", "tavily", "web", "lookup", "find", "internet", "extract", "crawl", "scrape", "research", "rag"]
author: "Tavily"
---

# Onboarding

Before using Tavily, you need a Tavily account. Sign up at [tavily.com](https://tavily.com) if you don't have one. Authentication is handled via OAuth — on first MCP connection, your browser will open for login. Tokens are cached in `~/.mcp-auth/`.

Alternatively, get an API key from your Tavily dashboard and set `TAVILY_API_KEY` in your environment.

# Overview

Tavily provides LLM-optimized APIs for web search, content extraction, site crawling, and AI-powered research. Results include relevance scores, content snippets, and metadata designed for agent consumption.

**Key capabilities:**
- **Search**: Web search with relevance scoring and domain/time filtering
- **Extract**: Pull content from specific URLs in markdown or text
- **Crawl**: Extract content from entire websites with configurable depth
- **Map**: Discover URLs on a site without extracting content
- **Research**: End-to-end AI-powered research from multiple sources

**Authentication**: OAuth via MCP server (auto-prompted on first use), or `TAVILY_API_KEY` environment variable.

## Available Steering Files

- **api-reference** — Full parameter reference for all 5 Tavily MCP tools
- **best-practices** — Query optimization, filtering strategies, common workflows

## Available MCP Servers

### tavily

**Connection:** HTTPS API endpoint at `https://mcp.tavily.com/mcp`
**Authorization:** OAuth (browser login on first use) or Bearer token via `TAVILY_API_KEY`

**Tools:**

1. **tavily_search** — Search the web for current information on any topic
   - Required: `query` (string)
   - Optional: `max_results` (default 5), `search_depth` ("basic"/"advanced"/"fast"/"ultra-fast"), `time_range` ("day"/"week"/"month"/"year"), `include_domains`, `exclude_domains`, `country`, `start_date`/`end_date` (YYYY-MM-DD), `include_raw_content`, `include_images`, `include_image_descriptions`, `include_favicon`
   - Returns: Results with title, url, content snippet, and relevance score

2. **tavily_extract** — Extract content from URLs in markdown or text
   - Required: `urls` (array of strings)
   - Optional: `extract_depth` ("basic"/"advanced"), `format` ("markdown"/"text"), `query` (reranks content chunks by relevance), `include_images`, `include_favicon`
   - Returns: Extracted content per URL, plus failed_results

3. **tavily_crawl** — Crawl a website starting from a URL
   - Required: `url` (string)
   - Optional: `max_depth` (default 1), `max_breadth` (default 20), `limit` (default 50), `instructions` (natural language guidance), `select_paths`, `select_domains`, `allow_external`, `extract_depth`, `format`, `include_favicon`
   - Returns: Array of pages with url and extracted content

4. **tavily_map** — Map a website's URL structure (no content extraction)
   - Required: `url` (string)
   - Optional: `max_depth` (default 1), `max_breadth` (default 20), `limit` (default 50), `instructions`, `select_paths`, `select_domains`, `allow_external`
   - Returns: Array of discovered URLs

5. **tavily_research** — Comprehensive multi-source research on a topic
   - Required: `input` (string) — description of the research task
   - Optional: `model` ("mini"/"pro"/"auto", default "auto")
   - Returns: Detailed research report with findings. Rate limit: 20 requests/minute

## MCP Configuration

```json
{
  "mcpServers": {
    "tavily": {
      "url": "https://mcp.tavily.com/mcp"
    }
  }
}
```
