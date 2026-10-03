# Grounded Web Search

> Search the web, then verify the results aren't lying.

A skill for AI coding assistants that enforces a simple discipline: **never trust the first confident result.** Search, then challenge what you found.

## Why this exists

LLMs confidently cite things that are wrong, outdated, or one-sided. We've all seen it:

* A "best practice" article is really just SEO content with no practitioner behind it
* Every result on the first page shares the same framing because they all cite the same source
* A technical claim that sounds authoritative but has no primary source backing it
* A library's API changed two major versions ago — the blog post from 2022 doesn't mention it

This skill exists to break that pattern. It's not about *finding* information — it's about **not getting fooled** by it.

## What it does

| Layer | What you get |
|-------|-------------|
| **Query construction** | How to write searches that actually return useful results (3–6 words for simple, full questions for research) |
| **Tool selection** | Which search tool to use for which job — broad search, deep fetch, async research, docs, crawling |
| **Source quality** | What to trust (official docs, Wikipedia, GitHub issues) vs. what to treat with caution (SEO listicles, undated articles, single-source claims) |
| **Challenge searches** | The core pattern: after finding something, actively search for counterarguments before presenting it as fact |
| **Parallel execution** | How to split a complex question into parallel sub-queries instead of sequential round-trips |
| **Output formatting** | How to present findings — most significant first, sources in footnotes, contradictions surfaced explicitly |

## The challenge-search pattern

The heart of this skill. After finding information that will inform a decision:

```
Original search:  "benefits of [X]"
Challenge search: "drawbacks of [X]" OR "problems with [X]" OR "[X] vs [alternative]"

Original search:  "[Claim] evidence"
Challenge search: "[Claim] debunked" OR "criticism of [claim]" OR "counterargument [claim]"
```

**How to weigh results:**

- Strong, credible counterpoints → surface them explicitly alongside the original finding
- Weak or fringe results → original finding is more reliable; note the absence of credible counterarguments
- Conflicting expert opinion → present both sides without forcing a conclusion

## Contributing

Found a pattern that works? A query tip that consistently returns better results? Open a PR.

## License

MIT
