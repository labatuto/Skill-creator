# Search Query Formulation Guide

The single biggest failure mode in AI search is **bad query formulation**. Models generate queries that sound like natural language questions or editorialized summaries — but search engines index *content*, not summaries. You need to search for words that would **actually appear on the page you're looking for**.

## The Core Principle

> **Think like an indexer, not a questioner.**
>
> Don't ask: "What is the current status of the EU AI Act?"
> Search for: `EU AI Act implementation timeline 2026`
>
> Don't ask: "Is coffee good or bad for your health?"
> Search for: `coffee cardiovascular risk meta-analysis`
>
> Don't ask: "What do experts think about the housing market?"
> Search for: `housing market forecast 2026 mortgage rates inventory`

The words in your query should be words that literally appear in articles, papers, and pages about this topic.

## Bad Query Patterns (AVOID THESE)

### 1. Natural Language Questions
- BAD: `"What are the side effects of metformin?"`
- GOOD: `metformin side effects clinical data`
- WHY: Pages don't typically contain the exact phrasing of your question. They contain the *answer*.

### 2. Overly Specific Exact Phrases
- BAD: `"the exact impact of tariffs on semiconductor supply chains in 2025"`
- GOOD: `semiconductor tariffs supply chain impact 2025`
- WHY: Nobody wrote that exact phrase. You're filtering out 99% of relevant results.

### 3. Editorialized/Opinionated Framing
- BAD: `"why is Python better than Java"`
- GOOD: `Python vs Java performance comparison benchmarks`
- WHY: You're embedding your assumption into the query. Search for the evidence, not the conclusion.

### 4. Overly Broad Single-Word Queries
- BAD: `cryptocurrency`
- GOOD: `cryptocurrency regulation SEC enforcement 2026`
- WHY: Too broad returns noise. Add specificity through domain terms, time bounds, or source types.

## Good Query Patterns (USE THESE)

### 1. Keyword Clusters
Combine 3-5 highly relevant keywords that would co-occur on the target page:
- `transformer architecture attention mechanism scaling laws`
- `CRISPR gene therapy clinical trials FDA approval`

### 2. Domain Jargon + Context
Use the terminology experts actually use:
- Instead of "how computers learn patterns": `machine learning gradient descent optimization`
- Instead of "new weight loss drugs": `GLP-1 receptor agonist semaglutide clinical outcomes`

### 3. Source-Targeted Queries
When you know the type of source you want:
- Academic: `[topic] site:arxiv.org` or `[topic] site:scholar.google.com`
- Technical: `[topic] site:github.com` or `[topic] documentation`
- News: `[topic] site:reuters.com` or `[topic] site:apnews.com`
- Discussion: `[topic] site:reddit.com` or `[topic] site:news.ycombinator.com`
- Government: `[topic] site:gov` or `[topic] site:europa.eu`

### 4. Time-Bounded Queries
For recency-sensitive topics, include the year or timeframe:
- `SpaceX Starship launch 2026`
- `federal reserve interest rate decision March 2026`

### 5. Contrastive Queries
When investigating disagreements, search for both sides:
- `[claim] evidence supporting` AND `[claim] criticism rebuttal`
- `[policy] benefits` AND `[policy] risks concerns`

### 6. Specific Claim Verification
When verifying a specific claim, search for its key components:
- Claim: "Tesla sold 2 million cars in 2025"
- Query: `Tesla vehicle deliveries 2025 annual sales figures`

### 7. Multi-Hop Seed Queries
When you need to find sources that will lead to other sources:
- `[topic] literature review` or `[topic] systematic review`
- `[topic] bibliography references`
- `[topic] "further reading"` or `[topic] "see also"`

## Query Reformulation Strategy

If your first query doesn't return good results:

1. **Broaden**: Remove the most specific term, keep the core concept
2. **Narrow**: Add a domain qualifier, date, or source type
3. **Rephrase**: Use synonyms or alternative terminology
4. **Pivot**: Search for a known expert or organization in this space, then find their content
5. **Reverse**: Search for the answer you expect, not the question

Example reformulation chain:
1. `quantum computing error correction breakthrough 2026` → too few results
2. `quantum error correction logical qubit` → better, more technical
3. `quantum error correction Google IBM 2026` → adds institutional context
4. `"quantum error correction" site:nature.com OR site:arxiv.org` → targets authoritative sources

## Per-Phase Query Guidance

### Phase 2 (Exploratory): Cast a Wide Net
- Use 2-3 word clusters, broad terms
- Include the topic + "overview" or "state of" or "latest"
- Try both technical and popular phrasings

### Phase 4 (Deep Dive): Precision Targeting
- Use full keyword clusters with domain jargon
- Target specific source types
- Include time bounds
- Use site-specific searches for known authoritative sources

### Phase 5 (Gap-Filling): Creative Approaches
- Try synonyms and alternative framings
- Search for experts/organizations rather than topics
- Look for meta-sources (reviews, bibliographies, link roundups)
- Try searching for the *opposite* claim to find rebuttals that address your gap
