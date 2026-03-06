---
name: megasearch
description: Deep multi-agent web research that thinks before searching, formulates expert queries, fans out parallel sub-agents, follows links aggressively, cross-references sources critically, and synthesizes findings with citations. Use when the user needs thorough, accurate, well-sourced answers to complex questions.
argument-hint: <query> [--format briefing|conversational|bullet]
allowed-tools: Read, WebSearch, WebFetch, Agent, Bash(echo *), Grep, Glob
---

# MegaSearch: Multi-Agent Deep Research

You are executing a deep research workflow. Your job is to produce the most accurate, well-sourced, critically-reasoned answer possible to the user's query.

**Query:** $ARGUMENTS

Parse the query for an optional `--format` flag:
- `--format briefing` — structured research briefing (default)
- `--format conversational` — natural prose with footnoted sources
- `--format bullet` — concise bullet-point summary with source links

If no `--format` is specified, use `briefing`.

---

## PHASE 1: Prior Knowledge Dump (THINK BEFORE YOU SEARCH)

**This phase is critical. Do NOT skip it.**

Before making ANY search or web request, think out loud about what you already know about this topic. Write out:

1. **What you know with high confidence** — established facts, core concepts, key figures
2. **What you think you know but aren't sure about** — things that might be outdated, contested, or misremembered
3. **What you don't know** — gaps in your knowledge, recent developments you'd be unaware of
4. **Your priors** — what answer do you *expect* to find? What would surprise you?

**Why this matters:** Search results can poison your reasoning. If you don't anchor your priors first, you'll uncritically absorb whatever the first result says. By writing down what you expect, you can notice when results contradict your priors (which is interesting and worth investigating) vs. when they confirm them (which should still be verified).

---

## PHASE 2: Exploratory Search (Map the Landscape)

Run 2-4 broad, exploratory searches to understand:
- What terminology is being used around this topic right now
- Who the key voices/sources are
- Whether there's active controversy or recent developments
- What adjacent topics connect to this

**Query formulation rules — READ CAREFULLY:**

See [query-strategy.md](query-strategy.md) for the full guide, but the key principles are:

1. **Search for words that would actually appear on the page**, not your editorialized summary of the topic. Think: "what exact words would a journalist/researcher/expert write in an article about this?"
2. **Use multiple phrasings** — the same concept gets described different ways
3. **Include domain-specific jargon** when searching for technical topics
4. **Use site-specific searches** when you know good sources (e.g., `site:arxiv.org`, `site:reddit.com`)
5. **NEVER use overly specific exact-match phrases** as your primary queries — cast a wider net first
6. **Include the current year** for topics where recency matters

For each search, note what you learned and what new questions emerged.

---

## PHASE 3: Strategy Design

Based on your prior knowledge (Phase 1) and exploratory findings (Phase 2), design your real search strategy. Write out:

1. **Key questions to answer** — decompose the query into 3-8 specific sub-questions
2. **Search vectors** — for each sub-question, what searches and sources would answer it?
3. **Source types needed** — do you need academic papers? News? Primary documents? Expert opinions? Government data?
4. **Contradictions to investigate** — did anything from Phase 2 contradict your priors? Investigate both sides.
5. **Multi-hop targets** — are there specific pages/sources you expect to contain links to deeper sources?

Assign each search vector a **prospect level**:
- **High prospect**: Directly addresses a core sub-question. Likely to yield authoritative sources.
- **Medium prospect**: Addresses a supporting question or provides context. May yield useful sources.
- **Low prospect / high variance**: Long shot, but could surface something unexpected. Worth checking but don't invest heavily.

---

## PHASE 4: Parallel Sub-Agent Execution

Now fan out your search strategy using parallel sub-agents. This is where the real work happens.

**Launch sub-agents using the Agent tool** with `subagent_type: "general-purpose"`. Run as many as needed in parallel (typically 3-6 agents).

Each sub-agent should receive a focused brief:
- The specific sub-question(s) it's investigating
- Suggested search queries (but empower it to reformulate)
- What source types to prioritize
- Instructions to **aggressively follow links** — when a source references another document, study, article, or dataset, fetch that linked source too
- Instructions to **capture full context**: direct quotes, URLs, dates, author names, publication names

**Sub-agent prompt template:**

```
You are a research sub-agent working on one piece of a larger investigation.

RESEARCH QUESTION: [specific sub-question]

SUGGESTED QUERIES (reformulate if these don't work well):
- [query 1]
- [query 2]

INSTRUCTIONS:
1. Run the suggested searches (and your own reformulations) using WebSearch
2. For each promising result, use WebFetch to read the full page
3. AGGRESSIVELY FOLLOW LINKS: When a source references another document, study, or article, fetch that too. Follow at least 2-3 hops deep when the links seem relevant.
4. For each source you read, extract:
   - Key claims and findings (with DIRECT QUOTES where possible)
   - Author/publication/date
   - URL
   - What other sources this source references or links to
   - Your assessment: is this a primary source, secondary reporting, opinion, or aggregation?
5. Note any contradictions between sources
6. Note what you could NOT find or verify

Return a structured research brief with all findings, quotes, and source metadata.
```

**Important:** Launch sub-agents for different search vectors in parallel (single message, multiple Agent tool calls). Don't serialize what can be parallelized.

For the low-prospect/high-variance vectors, you may group 2-3 into a single sub-agent to be efficient.

---

## PHASE 5: Review & Decide (Anti-Rabbithole Check)

Once sub-agents return, consolidate their findings. For each sub-question, assess:

1. **Coverage**: Do we have enough information to answer this confidently?
2. **Source quality**: Are we relying on primary sources or just secondary reporting?
3. **Agreement**: Do sources agree? If not, do we understand why?
4. **Gaps**: What's still missing?

**Decision point — be honest with yourself:**

- If coverage is good (>80% of sub-questions well-answered with quality sources) → proceed to Phase 6
- If there are critical gaps → design a targeted second pass (Phase 4b) hitting ONLY the gaps
- **Anti-rabbithole rule**: You get at most TWO passes. After the second pass, work with what you have. Signal low confidence on poorly-covered areas rather than endlessly searching.

**Second pass guidelines (if needed):**
- Only investigate genuine gaps, not "nice to have" deeper dives
- Use findings from first pass to formulate better queries
- Follow promising links that first-pass agents identified but didn't have time to explore
- Maximum 2-3 additional sub-agents

---

## PHASE 6: Source Curation & Cross-Referencing

Now do the critical reasoning work. This is where you earn the user's trust.

### 6a. Source Quality Assessment

For each source, evaluate:
- **Authority**: Who wrote this? What are their credentials? Is this a primary source?
- **Recency**: When was this published? Is it still current?
- **Corroboration**: Do other independent sources confirm this?
- **Bias**: Does this source have an obvious agenda? Is it trying to sell something?
- **Specificity**: Does this source make specific, verifiable claims or vague generalities?

### 6b. Cross-Reference Matrix

Build a mental model of what each source says about the key claims:
- Where do sources **agree**? (Higher confidence)
- Where do sources **disagree**? (Investigate why — is it a factual dispute, different time periods, different definitions?)
- What does source A say about source B's claims? (Recursive source evaluation)
- Which claims are supported by **only one source**? (Flag as lower confidence)

### 6c. Discard Bad Sources

Remove or explicitly flag:
- Sources that are clearly outdated
- Sources with obvious factual errors on verifiable claims
- Sources that are just aggregating/parroting other sources without adding value
- Sources with clear undisclosed conflicts of interest

### 6d. Organize Evidence

Group your vetted findings by sub-question/theme. For each group:
- Lead with the strongest evidence
- Include direct quotes for key claims
- Note confidence level (high/medium/low)
- Note where evidence is contradictory

---

## PHASE 7: Synthesis & Output

Generate the final output in the requested format.

### Format: `briefing` (default)

```markdown
# Research Briefing: [Topic]

## Executive Summary
[2-4 sentences capturing the key findings and overall confidence level]

## Key Findings

### [Finding 1 Title]
[Evidence and analysis with inline citations]
**Confidence:** High/Medium/Low
**Sources:** [numbered source list]

### [Finding 2 Title]
...

## Contradictions & Open Questions
[What sources disagree on, and what remains unclear]

## Source Appendix
[Numbered list of all sources with URL, author, date, and quality assessment]
```

### Format: `conversational`

Write a natural, flowing answer to the query with footnoted sources. Include a source appendix at the end. Use a confident but honest tone — signal where you're certain vs. where evidence is mixed.

### Format: `bullet`

```markdown
# [Topic]: Key Findings

- **[Finding 1]** — [brief evidence] ([Source](url))
- **[Finding 2]** — [brief evidence] ([Source](url))
- ...

## Caveats
- [What's uncertain or contested]

## Sources
- [Numbered source list with URLs]
```

---

## Meta-Rules (Apply Throughout All Phases)

1. **Never trust a single source.** Always seek corroboration.
2. **Prefer primary sources over secondary reporting.** A study > a news article about the study.
3. **Date everything.** A source from 2020 might be obsolete for fast-moving topics.
4. **Quote, don't paraphrase, for critical claims.** Paraphrasing introduces your biases.
5. **Be explicit about confidence levels.** "I found strong evidence that X" vs. "One source claims X but I couldn't verify it."
6. **If you can't find something, say so.** An honest "I couldn't find reliable information on X" is more valuable than a guess.
7. **Watch for circular sourcing.** Multiple articles may all trace back to the same original (often wrong) source.
8. **Search queries should match indexed content, not your editorial summary.** See [query-strategy.md](query-strategy.md).
