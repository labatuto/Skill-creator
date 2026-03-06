# Search Query Formulation Guide

The biggest failure mode in AI search is **bad queries**. Models write natural language questions — but search engines index *content*, not questions.

> **Think like an indexer, not a questioner.**
> Don't ask: "What is the current status of the EU AI Act?"
> Search for: `EU AI Act implementation timeline 2026`

## Bad Patterns (Avoid)

| Pattern | Bad | Good | Why |
|---------|-----|------|-----|
| Natural language questions | `"What are the side effects of metformin?"` | `metformin side effects clinical data` | Pages contain answers, not questions |
| Long exact phrases | `"the exact impact of tariffs on semiconductor supply chains"` | `semiconductor tariffs supply chain impact 2025` | Nobody wrote that phrase |
| Editorialized framing | `"why is Python better than Java"` | `Python vs Java performance benchmarks` | Don't embed your conclusion |
| Too broad | `cryptocurrency` | `cryptocurrency regulation SEC enforcement 2026` | Add domain terms, dates, source types |

## Good Patterns (Use)

**Keyword clusters** — 3-5 co-occurring terms: `transformer architecture attention mechanism scaling laws`

**Domain jargon** — expert terminology: `GLP-1 receptor agonist semaglutide clinical outcomes` not "new weight loss drugs"

**Source-targeted** — use `site:` for known good sources:
- Academic: `site:arxiv.org`, `site:scholar.google.com`
- News: `site:reuters.com`, `site:apnews.com`
- Technical: `site:github.com`
- Government: `site:gov`, `site:europa.eu`
- Discussion: `site:reddit.com`, `site:news.ycombinator.com`

**Time-bounded** — include year for recency-sensitive topics: `SpaceX Starship launch 2026`

**Contrastive** — search both sides: `[claim] evidence supporting` AND `[claim] criticism rebuttal`

**Verification** — search for claim components: claim "Tesla sold 2M cars in 2025" → `Tesla vehicle deliveries 2025 annual sales figures`

**Multi-hop seeds** — find sources that lead to other sources: `[topic] literature review`, `[topic] systematic review`

## Reformulation When Results Are Poor

1. **Broaden**: Remove the most specific term
2. **Narrow**: Add domain qualifier, date, or source type
3. **Rephrase**: Try synonyms or alternative terminology
4. **Pivot**: Search for known experts/organizations, then find their content
5. **Reverse**: Search for the expected answer, not the question

Example chain:
1. `quantum computing error correction breakthrough 2026` → too few results
2. `quantum error correction logical qubit` → more technical
3. `"quantum error correction" site:nature.com OR site:arxiv.org` → targeted sources

## Phase-Specific Guidance

- **Exploratory (Phase 2)**: 2-3 word clusters, broad terms, try technical + popular phrasings
- **Deep dive (Phase 4)**: Full keyword clusters, domain jargon, site-targeted, time-bounded
- **Gap-filling (Phase 5)**: Synonyms, search for experts/orgs, look for meta-sources and rebuttals
