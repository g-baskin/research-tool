---
name: deep-research-suite
version: 1.0.0
description: No-API deep research router for LLM agents: fast cited landscape reports plus lawful OSINT investigations with evidence grading, claim ledgers, tech-stack fingerprinting, and prior-art mapping.
---

# Deep Research Suite

Use this skill when the user asks for deep research, market research, company/domain investigation, public-source OSINT, tech-stack fingerprinting, competitor mapping, or IP/prior-art discovery.

This skill combines two research modes:

- **Landscape mode**: fast, multi-source synthesis for topics, decisions, comparisons, and current-state reports.
- **OSINT mode**: exhaustive lawful public-source investigation for a specific website, company, product, repository, idea, market, owner/operator, tech stack, competitor set, or prior-art landscape.

## Non-negotiable boundaries

- Use only lawful public sources: open web pages, public registries, public documents, public code/package registries, patent/trademark databases, DNS/certificate-transparency records, and pages reachable without bypassing access controls.
- Do not use hacking, credential access, exploit testing, login bypass, scraping behind authentication, rate-limit evasion, deception, private databases, breached/leaked data, or sealed/non-public records.
- Do not create private-person dossiers, stalking profiles, doxxing outputs, home addresses, private phone numbers, family details, routines, location tracking, or sensitive personal data.
- Public founder/operator research is allowed only for business context: legal entities, public executives, professional profiles, public filings, public press, trademarks, patents, and official pages.
- If a public record exposes irrelevant private details, omit those details and summarize only the business-relevant fact.
- Ask before using any paid service, account login, API key, high-volume scraping, active scanning, or questionable source.
- Passive technical fingerprinting is allowed; non-consensual vulnerability scanning or intrusive probing is not.

## Mode router

Choose the smallest mode that satisfies the request.

| User asks for | Use mode | Output |
|---|---|---|
| “Research X”, “deep dive”, “state of the market”, “compare A vs B” | Landscape | One cited Markdown report |
| “Help me decide”, “which should I choose”, “what are the tradeoffs” | Landscape + decision summary | Report plus recommendation matrix |
| “Find everything public about this company/domain/product” | OSINT | Full evidence folder |
| “Who owns this?”, “what company operates this?” | OSINT ownership track | Ownership/entities file plus claim ledger |
| “What tech stack do they use?” | OSINT tech-stack track | Confirmed vs inferred fingerprint |
| “Is this idea already out there?”, “prior art”, “patent/trademark leads” | OSINT prior-art track | Prior-art table and legal-review caveat |
| “Quick answer only” or “chat-only” | Lightweight landscape | Chat summary with sources |

## Tool stack

Prefer built-in agent tools first: web search, page fetch/readers, local shell commands, code search, repository inspection, and PDF/document readers if available.

If this repository is installed, the optional helper lives at:

```bash
~/.gg/skills/deep-research-suite/scripts/research
```

Use it for no-API DuckDuckGo search and full-page fetches:

```bash
~/.gg/skills/deep-research-suite/scripts/research --full \
  "<topic> overview 2026" \
  "<topic> market trends evidence" \
  "<topic> risks criticism alternatives" \
  --max 8 --fetch-top 3 --output research-raw.md
```

For a target investigation:

```bash
~/.gg/skills/deep-research-suite/scripts/research --full \
  "<target> official website company founders owner" \
  "<target> pricing docs API changelog careers" \
  "<target> patents trademarks prior art competitors" \
  --max 10 --fetch-top 5 --output research-raw.md
```

## Landscape mode workflow

Use this for broad research and decision support.

1. **Clarify the goal**: Ask up to two questions only when needed: “Is this for learning, a decision, or writing?” and “Any specific angle or depth?” If the user says “just research it,” proceed with reasonable defaults.
2. **Plan before searching**: Break the topic into 3-5 sub-questions.
3. **Search broadly**: Use 2-3 query variations per sub-question. Mix official, academic, reputable news, analyst, documentation, and community sources. Aim for 15-30 unique sources on substantial reports.
4. **Deep-read key sources**: Read 3-5 primary/high-signal sources in full. Do not rely only on search snippets.
5. **Synthesize**: Group findings into themes. Cross-check claims. Flag single-source claims as unverified.
6. **Deliver**: Save a report unless the user asked for chat-only.

Landscape report structure:

```markdown
# <Topic>: Deep Research Report
Generated: <date>
Sources: <count>
Confidence: <High/Medium/Low with reason>

## Executive Summary
## Key Findings
## 1. <Major Theme>
## 2. <Major Theme>
## 3. <Major Theme>
## Decision Notes / Tradeoffs
## Key Takeaways
## Sources
## Methodology
## Gaps and Unknowns
```

## OSINT mode workflow

Use this for target-specific public-source investigations.

1. **Scope and safety check**: Identify target type: website/domain, company, product, open-source project, idea/IP concept, market, tech stack, or mixed target. State safety limits only when the request approaches private-person, sealed-record, exploit, credential, or active-scanning territory.
2. **Build the research map**: Create tracks for official sources, ownership/legal, technical footprint, market/competition, IP/prior art, public business roles, and negative/contradiction searches.
3. **Search broadly, then drill down**: Use many small searches: exact names, quoted phrases, domain searches, founder/entity terms, filetype searches, date-bounded searches, and target-specific operators.
4. **Website/domain pass**: Collect canonical domain, redirects, public subdomains from CT/search, `robots.txt`, `sitemap.xml`, docs, help center, changelog, status, API references, headers, visible source clues, package links, and archive/history.
5. **Ownership/entity pass**: Distinguish legal owner, brand operator, parent company, public founders/executives, maintainers, domain registrant if public/business-relevant, and funding/acquisition signals.
6. **Tech-stack pass**: Separate confirmed signals from inferred signals. Confirmed means direct evidence from docs, repos, manifests, SDKs, headers, public status pages, engineering posts, or job descriptions. Inferred means class names, build artifacts, cookies, CDN paths, analytics IDs, or circumstantial clues.
7. **IP/prior-art pass**: Decompose the idea into 5-10 claims/features/workflows. Search patents, trademarks, app stores, Product Hunt, GitHub, package registries, papers, standards, forums, and archived pages. Do not provide legal conclusions.
8. **Contradiction/freshness pass**: Prefer current official/legal sources over stale profiles. Re-run negative searches such as `"<target>" lawsuit`, complaint, acquisition, shutdown, outage, or data breach when appropriate.
9. **Deliver evidence artifacts**: Save a folder with source log, claim ledger, findings, unknowns, and relevant track files.

OSINT artifact folder:

```text
research-reports/<slug>/
├── DEEP_OSINT_REPORT.md
├── SOURCES.csv
├── QUERY_LOG.md
├── CLAIMS_LEDGER.md
├── UNKNOWNS_AND_NEXT_LEADS.md
├── OWNERSHIP_AND_ENTITIES.md          # if relevant
├── TECHSTACK_FINGERPRINT.md           # if relevant
├── IP_PRIOR_ART.md                    # if relevant
└── COMPETITIVE_LANDSCAPE.md           # if relevant
```

## Evidence grading

Use these grades for substantive OSINT claims:

- **A — Primary source**: official page, filing, patent/trademark record, public registry, public code/package metadata, SEC/regulatory filing, direct interview/transcript.
- **B — Reputable secondary source**: established news, reputable industry database/page, conference bio, well-maintained directory with citations.
- **C — Weak secondary/source snippet**: blog/forum/review/social post, search snippet, unaudited database, old source, or single uncorroborated mention.
- **D — Inference**: technical fingerprinting or circumstantial evidence; useful lead, not fact.

Every substantive OSINT claim must include source IDs, evidence grade, confidence, contradictions, and freshness. Unsupported claims belong in `UNKNOWNS_AND_NEXT_LEADS.md`, not findings.

## Quality rules

- Every important claim needs a source.
- Cross-reference important claims; flag single-source findings.
- Prefer sources from the last 12 months unless history is the point.
- Record accessed dates for OSINT artifacts.
- Say “insufficient data found” instead of guessing.
- Separate confirmed facts, inferences, and open leads.
- Do not dump a long report into chat; return the path, highest-signal findings, unknowns, and recommended second pass.

## Final chat response format

For long reports, respond with:

```markdown
Done: <report folder or report file path>

Highest-signal findings:
- ...

Major unknowns / limits:
- ...

Not searched:
- <category and why, if any>

Want a second pass on ownership, tech stack, competitors, or IP/prior art?
```
