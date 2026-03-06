---
name: megasearch
description: Deep multi-agent web research that thinks before searching, formulates expert queries, fans out parallel sub-agents, follows links aggressively, cross-references sources critically, and synthesizes findings with citations. Use when the user needs thorough, accurate, well-sourced answers to complex questions.
argument-hint: <query> [--format briefing|conversational|bullet]
allowed-tools: Read, WebSearch, WebFetch, Agent, Bash(echo *), Grep, Glob
---

# MegaSearch: Multi-Agent Deep Research

You are executing a deep research workflow. Produce the most accurate, well-sourced, critically-reasoned answer possible.

**Query:** $ARGUMENTS

Parse for `--format` flag: `briefing` (default), `conversational`, or `bullet`.

---

## PHASE 0: Complexity Triage

Before anything else, classify the query:

**Simple** (single fact, well-known topic, likely answered by 1-2 searches):
→ Skip to a streamlined path: do 2 searches, verify the answer across sources, output with citations. No sub-agents needed.

**Moderate** (multi-faceted but well-documented topic, 2-4 sub-questions):
→ Run Phases 1-7 but limit to 2-3 sub-agents, skip second pass.

**Deep** (complex, contested, multi-domain, or poorly-documented topic):
→ Full workflow with 3-6 sub-agents, second pass if needed.

State your classification and reasoning in one sentence, then proceed accordingly.

---

## PHASE 1: Prior Knowledge Anchor

**Do NOT skip this.** Before any search, briefly note:

1. **What you know** — established facts, key concepts
2. **What you're unsure about** — potentially outdated or contested
3. **What you don't know** — gaps, recent developments
4. **Your expected answer** — so you can notice when results contradict your priors

Keep this to ~5-10 lines. The goal is anchoring your priors to resist search result poisoning, not writing an essay.

---

## PHASE 2: Exploratory Search

Run 2-4 broad searches **in parallel** to map the landscape:
- Current terminology and key voices
- Active controversy or recent developments
- Adjacent topics

**Query rules** (critical — also applies to sub-agents):
1. **Search for words that would appear on the target page**, not your question about it. Think like an indexer.
2. **Use keyword clusters** (3-5 terms): `GLP-1 receptor agonist clinical outcomes` not `"are the new weight loss drugs safe?"`
3. **Use domain jargon** for technical topics
4. **Include the year** for recency-sensitive topics
5. **Never use long exact phrases** as primary queries — cast a wide net first
6. **Use site: operators** for known good sources (`site:arxiv.org`, `site:reuters.com`)

See [query-strategy.md](query-strategy.md) for the full reformulation guide.

Briefly note what you learned and what new questions emerged.

---

## PHASE 3: Strategy → Sub-Agent Launch (Combined)

Design your search strategy and **immediately launch sub-agents in the same step**. Don't write a strategy document and then launch separately — combine them.

For each sub-question, determine:
- What searches and source types would answer it
- Priority: **high** (core question), **medium** (supporting), **low/high-variance** (long shot)

Then launch sub-agents in parallel. Group low-priority vectors into a single agent.

---

## PHASE 4: Sub-Agent Execution

Launch sub-agents using `Agent` tool with `subagent_type: "general-purpose"`. Run all in a single message for parallelism.

**Sub-agent prompt template:**

```
You are a research sub-agent. Your findings will be combined with other agents' work.

RESEARCH QUESTION: [specific sub-question]

SUGGESTED QUERIES (reformulate if needed):
- [query 1]
- [query 2]

QUERY FORMULATION RULES (critical — follow these strictly):
- Search for words that would APPEAR ON THE TARGET PAGE, not your question about it
- Use keyword clusters (3-5 terms): "CRISPR gene therapy FDA approval 2026" not "what's happening with gene editing?"
- Use domain-specific jargon experts would use
- Include the year for recency-sensitive topics
- Never search for long exact phrases
- Use site: operators for authoritative sources (site:arxiv.org, site:reuters.com, site:gov)
- If a query returns poor results: broaden (remove specific terms), rephrase (try synonyms), or pivot (search for known experts/orgs)

RESEARCH INSTRUCTIONS:
1. Run 3-5 searches using WebSearch (suggested + your own reformulations)
2. Use WebFetch to read the 3-5 most promising results
3. Follow links when a source references key studies, documents, or data — go 1-2 hops deep on the most relevant
4. If WebFetch fails (paywall, JS-only, 404), note it and move on — don't retry
5. For each source, extract per the format below

OUTPUT FORMAT (follow exactly):
For each source, return:
---
**Source [N]:** [Title]
- **URL:** [url]
- **Author/Publication:** [who] | **Date:** [when]
- **Type:** primary / secondary / tertiary / opinion
- **Key claims:** [2-4 bullet points, use DIRECT QUOTES for critical claims]
- **References:** [other sources this links to that seem important]
---

Then provide:
- **Summary:** [3-5 sentences synthesizing what you found]
- **Contradictions:** [any disagreements between sources]
- **Gaps:** [what you looked for but couldn't find]

BUDGET: Return at most 1500 words total. Prioritize quality over quantity.
```

**Adapt the template** per sub-question — change the research question, queries, and any specific source types to prioritize.

---

## PHASE 5: Consolidation & Gap Check

Once sub-agents return, assess per sub-question:
- **Coverage**: enough info to answer confidently?
- **Source quality**: primary sources or just secondary reporting?
- **Agreement**: do sources agree? If not, why?

**Decision:**
- Good coverage → proceed to Phase 6
- Critical gaps → launch a targeted second pass (max 2-3 agents, only for genuine gaps)
- **Hard limit**: two passes maximum. After that, work with what you have and flag low-confidence areas.

**Deduplicate**: If multiple agents found the same source, consolidate into one entry. Note which agents independently found it (independent discovery = higher confidence).

---

## PHASE 6: Cross-Reference & Curate

Focus your critical analysis on the **3-5 most important or contested claims**. For each:

1. **Independence check**: Do agreeing sources trace back to the same original? (Multiple articles citing one press release = one source, not three)
2. **Authority**: Is this a primary source or telephone-game reporting?
3. **Recency**: Is this current enough for the topic?
4. **Contradiction resolution**: If sources disagree, check dates, definitions, methodology, or scope differences

Discard sources that are clearly outdated, factually wrong on verifiable claims, or just parroting other sources without adding value.

See [source-evaluation.md](source-evaluation.md) for the full evaluation framework.

---

## PHASE 7: Synthesis & Output

### Format: `briefing` (default)

```markdown
# Research Briefing: [Topic]

## Executive Summary
[2-4 sentences: key findings + overall confidence level]

## Key Findings

### [Finding 1]
[Evidence with inline citations]
**Confidence:** High/Medium/Low
**Sources:** [numbered]

### [Finding 2]
...

## Contradictions & Open Questions
[What sources disagree on, what remains unclear]

## Sources
[Numbered list: URL, author, date, one-line quality note]
```

### Format: `conversational`
Natural prose with footnoted sources. Confident but honest tone — signal certainty vs. mixed evidence. Source appendix at end.

### Format: `bullet`
```markdown
# [Topic]: Key Findings
- **[Finding]** — [evidence] ([Source](url))
- ...

## Caveats
- [What's uncertain]

## Sources
- [Numbered list with URLs]
```

---

## Meta-Rules

1. **Never trust a single source.** Seek corroboration.
2. **Prefer primary sources** over secondary reporting.
3. **Date everything.** Flag potentially outdated sources.
4. **Quote critical claims directly.** Paraphrasing introduces bias.
5. **Be explicit about confidence.** "Strong evidence" vs. "one unverified source claims."
6. **Say when you can't find something.** Honest gaps > confident guesses.
7. **Watch for circular sourcing.** Multiple articles can trace to one (often wrong) origin.
8. **Match queries to indexed content.** See [query-strategy.md](query-strategy.md).
