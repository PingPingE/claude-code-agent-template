---
name: fact-check
description: Verify factual claims in content
user-invocable: true
context: fork
agent: fact-checker
allowed-tools:
  - Read
  - Grep
  - Glob
  - WebSearch
---

# Fact Check

Verify factual claims in:

**Target:** $ARGUMENTS

## Check every:
1. Statistic and number
2. Factual claim
3. Attribution and quote
4. Technical description
5. Referenced link

Report as: verified / unverified / incorrect with sources.

## Verification Pipeline

### Step 1: Extract Claims
Read the content and identify every factual statement. Categorize as:
- **Statistics** — percentages, numbers, survey results
- **Historical facts** — dates, events, names
- **Quotes** — attributed statements
- **Technical claims** — API behavior, code functionality, software capabilities
- **Predictions** — future-oriented statements

### Step 2: Classify Each Claim
For each claim, determine:
- **Claim type** — which category above?
- **Verifiability** — can this be fact-checked?
- **Priority** — how important is accuracy for this claim?

### Step 3: Verify Each Claim
Use WebSearch to find:
1. **Primary sources** — original research, official docs, direct data
2. **Secondary sources** — peer-reviewed articles, reputable news
3. **Tertiary sources** — Wikipedia (starting point only, not final source)

Avoid: blog posts, social media, unattributed aggregators

### Step 4: Report Findings
For each claim, document:
- Location in content
- Status (Verified/Likely/Unverified/Incorrect)
- Source URL
- Notes on any issues

## Source Quality Tiers

**Tier 1 (Best):** Official documentation, peer-reviewed research, government data, primary sources
**Tier 2 (Good):** Reputable news outlets, industry reports, verified expert statements
**Tier 3 (Starting point):** Wikipedia, aggregators (verify with Tier 1/2 sources)
**Tier 4 (Avoid):** Blogs, social media, opinion pieces, uncited claims

## Confidence Levels

- **Verified** — found authoritative source, claim is accurate
- **Likely** — multiple indirect sources confirm, but no primary source found
- **Unverified** — cannot find source to confirm or deny
- **Incorrect** — found authoritative source that contradicts claim

## Report Format

| Claim | Location | Type | Status | Source | Notes |
|-------|----------|------|--------|--------|-------|
| "75% of developers use Git" | Para 2 | Statistic | Verified | Stack Overflow Developer Survey 2025 | Current and accurate |
| "Einstein said innovation requires..." | Intro | Quote | Incorrect | Quote Investigator | Misattribution — actually Mark Twain |
| "Next.js 15 uses React 19" | Para 5 | Technical | Verified | Next.js official docs | Accurate as of Feb 2026 |
| "80% of startups fail in year 1" | Para 3 | Statistic | Unverified | No primary source found | Common claim but unsubstantiated |

### Summary Statistics
- **Total claims checked:** X
- **Verified:** Y (Z%)
- **Likely:** Y (Z%)
- **Unverified:** Y (Z%)
- **Incorrect:** Y (Z%)

### Action Required

**Critical (must fix):**
1. [Incorrect claim] — replace with: [correct information] from [source]
2. [Another incorrect claim] — replace with: [correct information]

**Recommended (add sources):**
1. [Unverified claim] — add citation to: [suggested source]
2. [Another unverified claim] — add citation or remove claim

**Low priority:**
1. [Outdated but not wrong] — consider updating to more recent data

## Quality Checks

Before submitting report:
- [ ] Checked every statistic and number
- [ ] Verified all quotes and attributions
- [ ] Tested all URLs (no 404s)
- [ ] Flagged outdated information (2+ years old)
- [ ] Provided source URLs for all verified claims
- [ ] Offered correct information for all incorrect claims
