---
inclusion: manual
---

# Tavily Best Practices

## Search

- **Keep queries concise.** Think search query, not prompt.
- **Break complex queries into sub-queries.** Multiple focused searches beat one giant query.
- **Use `search_depth: "advanced"`** when precision matters — still fast, much better relevance.
- **Use `include_domains`** to restrict to trusted sources (e.g. `["arxiv.org", "github.com"]`).
- **Use `time_range`** for recent information (`"day"`, `"week"`, `"month"`, `"year"`).
- **Filter by `score`** (0–1) — discard results below 0.5 for higher quality.
- **Use `country`** with full country names (e.g. `"United States"`, not `"us"`).

## Extract

- **Two-step pattern**: Search first, then extract from the best URLs. More control than `include_raw_content` on search.
- **Use `query`** to rerank extracted chunks by relevance — returns the most useful parts first.
- **If `basic` fails**, retry with `extract_depth: "advanced"` — handles LinkedIn, protected sites, tables, and JS-rendered pages.
- **Use `"advanced"` extract_depth** proactively for sites you know are dynamic or protected.

## Crawl

- **Start conservative**: `max_depth=1`, `max_breadth=20`. Scale up only if needed.
- **Always set `limit`** to prevent runaway crawls and unexpected costs.
- **Use `instructions`** for semantic focus — the crawler prioritizes pages matching your description.
- **Use `select_paths`** when you know the site structure (e.g. `["/docs/.*", "/api/.*"]`).
- **Use `select_domains`** to stay within a subdomain (e.g. `["^docs\\.example\\.com$"]`).

## Map

- **Use Map before Crawl** when you don't know a site's structure.
- **Map → filter → Extract** is more precise than a blind crawl:
  1. `tavily_map` to discover URLs
  2. Filter the results to relevant paths
  3. `tavily_extract` on the filtered URLs

## Research

- **Be specific.** Include known details: target market, competitors, geography, constraints.
- **Share what you already know.** Prevents the research from repeating existing knowledge.
- **Use `"mini"`** for focused single-topic questions. **Use `"pro"`** for broad multi-domain analysis.
- **Rate limited** to 20 requests/minute — don't fire many in parallel.

## Common Workflows

### Find and read specific content
1. `tavily_search` with relevant query
2. Pick top results by score
3. `tavily_extract` on those URLs with a targeting `query`

### Build a knowledge base from a docs site
1. `tavily_map` to discover all pages
2. Filter to relevant paths
3. `tavily_extract` in batches

### Research a topic end-to-end
1. `tavily_research` with a clear, detailed prompt and appropriate model
2. Use the report and sources directly

### Get current information
1. `tavily_search` with `time_range: "week"` or `"day"`
2. Use `search_depth: "advanced"` for best relevance
