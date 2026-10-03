---
name: grounded-web-search
description: >
  Search the web, then verify the results aren't lying. Use this skill whenever performing
  any web search task - how to structure queries, how many sources to consult, and how to
  validate information through challenge searches. Triggers on any research task,
  fact-checking request, technical query, competitor analysis, news lookup, or any question
  where external web information is needed. Always consult this skill before picking a
  search tool.
---

# Grounded Web Search

Guidance for structuring queries and validating information through challenge searches.

---

## Tool Reference

**Note:** Not all tools might be available. This reference documents **known official
first-party MCP search servers**. Before searching, check what exists on the current MCP
gateway and only use the tools that are actually there.

### Roles → preferred tools

| Role | Best fit | Also good | Why |
|------|----------|-----------|-----|
| Broad web search | Google Search, Brave, Bing | Tavily, Exa, Linkup | General, privacy-first, news |
| Semantic/neural search | Exa | Tavily (basic), Linkup | Finds by meaning, not keywords |
| Full-text / deep content | Tavily, Linkup | Exa (contents), Firecrawl | Scored results, domain filters |
| Fetch a known URL | Jina Reader, Tavily Extract | Firecrawl Scrape, Exa (contents) | Quick markdown, JS-heavy sites |
| Async multi-source synthesis | Linkup Research, Tavily Research | Exa agent_run, Firecrawl /agent, Parallel Task API | Multi-source investigation |
| Site crawling / structure map | Firecrawl Map, Tavily Map | Exa (via contents) | |
| News | Brave News, Bing News | Tavily (news topic) | |
| Academic / PDF research | Jina (arXiv/SSRN/PDF) | Valyu (PubMed, arXiv, full-text) | |
| Structured data extraction | Firecrawl Extract | Tavily Extract, Linkup Extract | LLM-powered JSON schema extraction |
| Privacy-focused | Brave, Kagi | | Independent index |
| Library / framework / API docs | Context7 | | |
| GitHub repo structure & architecture | DeepWiki | | |
| Answer-first / cited answers | Perplexity Sonar, You.com (Research) | Parallel Search | One-call cited, web-grounded answers |
| Proprietary / specialized data | Valyu (SEC, PubMed, patents, clinical trials) | | Beyond public web — regulated industries |
| Multi-engine SERP | SerpAPI, Serper | SearchAPI.io, DataForSEO | Structured results from Google, Bing, etc. |
| Model-provider-native search | OpenAI Web Search, xAI/Grok | | Built into model API — no separate key |
| Open-source / self-hosted | Crawl4AI, fastCRW, TinySearch | | Self-hosted, free, privacy-focused |
| MCP-native budget search | Keiro, TinyFish | | Agent-optimized, low-cost |
| Dataset building at scale | Parallel Find All, Valyu | | Training data, market mapping |

### Provider strengths

- **Google Search** - broadest index, community/forum pushback
- **Bing Search** - web/news/image, market localization
- **OpenAI Web Search** - Built into Responses API — no separate key needed
- **xAI/Grok Web Search** - Built into Grok API — includes X/Twitter search, image understanding
- **Brave Search** - independent index (30B+ pages), privacy-first, LLM Context API (pre-extracted content), Goggles ranking
- **Tavily** - AI-optimized, 4 search depths, domain filters, scored results with content, RAG workflows. Full retrieval stack: Search + Extract + Crawl + Map + Research in one API
- **Exa** - neural/semantic search (meaning not keywords), clean markdown, advanced filters, multi-step research agent. Returns full page content by default
- **Firecrawl** - JS rendering, anti-bot bypass, clean markdown/JSON, LLM-powered structured extraction. Also has /search and /agent endpoints
- **Jina AI** - URL-to-markdown, academic search (arXiv/SSRN), PDF extraction with figures/tables, embeddings/reranking
- **Perplexity Sonar** - Answer-first retrieval with citations. Returns cited, web-grounded answer in one call
- **Context7** - library/framework/API documentation, version-specific docs
- **DeepWiki** - GitHub repo structure and architecture
- **Linkup** - full-text depth, async multi-source research synthesis. Accuracy-critical production systems
- **You.com** - 93% SimpleQA accuracy, cited research answers, finance index
- **Parallel AI** - Proprietary web index built for AI agents. Search + Task API (deep research) + Find All (dataset building). Token-efficient excerpts
- **Valyu** - Unified API for web + proprietary sources (SEC filings, PubMed, arXiv, clinical trials, USPTO patents, FRED economic data). 94% SimpleQA
- **SerpAPI** - 40+ search engines, enterprise-grade structured SERP data
- **Serper** - Google-only search API, clean structured results
- **SearchAPI.io** - Multi-engine SERP API (Google, Bing, Baidu, etc.)
- **DataForSEO** - Multi-engine SERP API, high-volume structured data
- **Kagi** - high-quality results, privacy-preserving, lenses (custom filters)
- **TinyFish** - Search + extraction + page operations (including behind-login). MCP-native
- **Keiro** - MCP-native search API, agent-optimized
- **Crawl4AI** - Open-source web crawler for AI agents
- **fastCRW** - Open-source search + scrape + answer API
- **TinySearch** - Local search backend for privacy-focused agents
- **Built in web search tools like `web_search` or `search_web`** - usually similar to **Bing Search**

---

## Layered Search Strategy

Start simple, escalate to depth and validation as needed:

```
1. Basic search       → Quick fact, breaking news, official info
2. Deep search        → Need more content, full-text depth
3. Research           → Need synthesis across many sources (async - poll get-research)
```

---

## Patterns, Tips and Procedures

### Query Construction Tips

- Keep queries **3–6 words** for a simple search they perform best on concise, keyword-style queries
- Use **full natural language questions** for research
- When reformulating a failed query, **change the terms meaningfully** - don't just rephrase the same words
- For docs questions, include the **version number** (e.g., "React 19" not "React") - context7 and search both benefit

### Source Quality Signals

**Prefer:**
- Official documentation and release notes
- Author-attributed technical articles on known publications
- Wikipedia (for factual/historical queries)
- GitHub issues/discussions (for technical problems)
- context7 / deepwiki / github content over blog posts when they cover the same API surface

**Treat with caution:**
- SEO-heavy "best of" listicles
- Undated articles on fast-moving topics
- Single-source claims on controversial topics
- Results that all share the same framing (signal: run a challenge search)

### Challenge Search (Validation Pattern)

**What it is:** After finding information that will inform a decision or recommendation, actively search for sources that contradict, challenge, or complicate the original finding. The goal is to avoid confirmation bias and surface dissenting perspectives before presenting a conclusion.

**When to use it:**
- Any claim that will be used to support a recommendation
- Competitive or market intelligence
- Technical decisions (library choice, architecture, tools)
- News or events with potential bias (controversial topics, company announcements)
- Any time results from one source feel too clean or one-sided

**How to run it:**

```
Original search:  "benefits of [X]"
Challenge search: "drawbacks of [X]" OR "problems with [X]" OR "[X] vs [alternative]"

Original search:  "[Product] positive reviews"
Challenge search: "[Product] complaints" OR "[Product] criticism" OR "why [Product] failed"

Original search:  "[Claim] evidence"
Challenge search: "[Claim] debunked" OR "criticism of [claim]" OR "counterargument [claim]"
```

**Recommended tool for challenge searches:** `google_search_search` (community/forum pushback), then `linkup_linkup-search` if full-text counterarguments are needed.

**How to weigh results:**
- If challenge search returns strong, credible counterpoints → surface them explicitly alongside the original finding
- If challenge search returns weak or fringe results → original finding is more reliable; note the absence of credible counterarguments
- If challenge search returns conflicting expert opinion → present both sides without forcing a conclusion

**Output format when challenge search finds meaningful contradictions:**

> ✅ **Original finding:** [summary]
> ⚠️ **Counterpoint found:** [summary of challenge result, with source]
> ⚖️ **Weight:** [your assessment of which is more credible and why]

### Parallel Execution

When a query has multiple distinct sub-questions, run searches in parallel rather than sequentially. Example:

```
User: "Compare Remix and Next.js for a project"

Run in parallel:
→ search: "Remix framework strengths"
→ search: "Next.js strengths"
→ search: "Remix vs Next.js Reddit" ← community sentiment

Then challenge:
→ search: "Remix drawbacks problems"
→ search: "Next.js drawbacks problems"
```

---

## Search Methodology

### Basic Search
For news, simple topics and official information
- Discover and execute three or five search queries using the Preferred Tools

### Deep Search
For complex topics, technical details and fact-checking
- Parallel execution for the main topics
- After each iterarion evaluate the answer. Ask yourself: "those the information uncovered provide insight into the user answer?"
- If the answer from the above question is "No", iterate
- Iterate no more than three times, then proceed with the answer

### Research
For literature reviews, deep investigations, or multi-source synthesis:
1. **Scope** - clarify domain, depth (5/15/30 sources), focus, and audience before searching
2. **Three-cycle search** - academic databases → authors & journals → citation trails (follow references and citations of key papers)
3. **Evaluate sources** - rate relevance, authority, recency, and type; prefer primary sources and peer-reviewed work
4. **Triangulate** - cross-reference for consensus, debates, gaps, and key figures; never present a single source as "the answer"
5. **Fact-check** - verify every citation exists (title, author, year, URL/DOI); delete unverifiable ones; AI systems fabricate citations 15-55% of the time
6. **Use existing patterns** - run challenge searches to validate key claims, parallel execution for sub-questions

## Output

### Simple search
- Most accurate and significant finds first
- Return all the links to the significant sources in the footnotes

### Complex search/research
- Most accurate and significant finds first, linked to the primary sources
- Summarize and reason the significant finds before proceeding with the less significant information
- key findings with sources, consensus, debates & open questions, full source list
- You can use tables for structured and tabular information and ASCII diagrams as needed
- Proceed with the rest of the less significant information if any. Be brief
- Return all the links to the significant sources in the footnotes

