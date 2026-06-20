# Research Tool

A no-API research skill for LLM agents that combines fast cited landscape research with lawful public-source OSINT investigations.

Use it when you want an agent to research a topic, compare options, investigate a public company/domain/product, fingerprint a public tech stack, map competitors, or find IP/prior-art leads without paid research APIs.

## What you get

- **Landscape research** for topics, decisions, comparisons, and market overviews.
- **OSINT research** for public company/domain/product investigations.
- **Evidence discipline** with source grading, claim ledgers, contradictions, and unknowns.
- **Tech-stack fingerprinting** that separates confirmed signals from inferences.
- **Prior-art mapping** for ideas, products, patents, trademarks, repos, packages, and launch history.
- **No paid APIs required**; the included helper uses DuckDuckGo through `ddgs`.

## Quickstart

Requires Python 3.11+ and [`uv`](https://docs.astral.sh/uv/getting-started/installation/).

```bash
git clone https://github.com/g-baskin/research-tool.git
cd research-tool
export SKILL_DIR="$HOME/.local/share/llm-skills"
mkdir -p "$SKILL_DIR"
cp -R skills/deep-research-suite "$SKILL_DIR/"
chmod +x "$SKILL_DIR/deep-research-suite/scripts/research"
"$SKILL_DIR/deep-research-suite/scripts/research" --help
```

If your LLM tool has its own skill directory, copy `skills/deep-research-suite/SKILL.md` there instead and tell the agent where the helper script lives.

## Claude Code manual install

The skill folder in this repo is:

```text
skills/deep-research-suite/
├── SKILL.md
└── scripts/
    └── research
```

For a personal Claude Code install, copy the whole folder into Claude Code's skills directory:

```bash
mkdir -p ~/.claude/skills
cp -R skills/deep-research-suite ~/.claude/skills/
chmod +x ~/.claude/skills/deep-research-suite/scripts/research
```

Verify the installed files:

```bash
ls ~/.claude/skills/deep-research-suite/SKILL.md
~/.claude/skills/deep-research-suite/scripts/research --help
```

If `~/.claude/skills/` did not exist when Claude Code started, restart Claude Code so it watches the new skills folder. Then invoke the skill with `/deep-research-suite` or ask for deep research.

## How to use it as an end user

After installation, ask your LLM naturally:

```text
Use deep-research-suite to research the current state of open-source CRM tools in 2026. I need a decision memo with tradeoffs and sources.
```

```text
Use deep-research-suite to run lawful public-source OSINT on example.com. Focus on ownership, product surface, public tech-stack signals, competitors, and unknowns. Save the evidence folder.
```

```text
Use deep-research-suite to map prior art for: an AI assistant that turns customer-support tickets into product-roadmap clusters. Include GitHub, Product Hunt, patents/trademarks, and SaaS competitors. This is research only, not legal advice.
```

```text
Use deep-research-suite in chat-only mode. Compare Rust vs Go for backend services in 2026 with 8-12 sources and a recommendation for a small SaaS team.
```

## Mode guide

| Request | Mode | Expected output |
|---|---|---|
| “Research this topic” | Landscape | Cited Markdown report |
| “Compare these tools” | Landscape decision memo | Recommendation matrix + sources |
| “Find everything public about this company/domain” | OSINT | Evidence folder |
| “Who owns/operators this?” | OSINT ownership track | Entity notes + claim ledger |
| “What tech stack do they use?” | OSINT tech-stack track | Confirmed vs inferred signals |
| “Is this idea already out there?” | OSINT prior-art track | Prior-art table + legal-review caveat |

## Helper script examples

The helper is optional; the skill can also use your LLM's built-in web search/fetch tools. Use the helper when your agent needs no-API search from a terminal.

```bash
# General landscape research
$SKILL_DIR/deep-research-suite/scripts/research --full \
  "open-source CRM tools 2026 comparison" \
  "open-source CRM GitHub active projects" \
  "open-source CRM pricing hosted alternatives" \
  --max 8 --fetch-top 3 --output research-raw.md
```

```bash
# Public company/domain investigation
$SKILL_DIR/deep-research-suite/scripts/research --full \
  "example.com official company founders owner" \
  "example.com pricing docs API changelog careers" \
  "example.com competitors reviews complaints" \
  --max 10 --fetch-top 5 --output osint-raw.md
```

```bash
# Fetch known URLs for deeper reading
$SKILL_DIR/deep-research-suite/scripts/research --fetch \
  "https://example.com/pricing" \
  "https://example.com/terms"
```

## Output folders

Landscape mode usually writes one report:

```text
research-reports/<slug>/report.md
```

OSINT mode writes an evidence folder:

```text
research-reports/<slug>/
├── DEEP_OSINT_REPORT.md
├── SOURCES.csv
├── QUERY_LOG.md
├── CLAIMS_LEDGER.md
├── UNKNOWNS_AND_NEXT_LEADS.md
├── OWNERSHIP_AND_ENTITIES.md
├── TECHSTACK_FINGERPRINT.md
├── IP_PRIOR_ART.md
└── COMPETITIVE_LANDSCAPE.md
```

Not every OSINT investigation needs every file, but `DEEP_OSINT_REPORT.md`, `SOURCES.csv`, `QUERY_LOG.md`, `CLAIMS_LEDGER.md`, and `UNKNOWNS_AND_NEXT_LEADS.md` should always exist.

## Safety rules

This tool is for lawful public-source research only. Do not use it for hacking, exploit testing, login bypass, scraping private/authenticated areas, rate-limit evasion, private-person dossiers, doxxing, leaked data, sealed records, or intrusive scanning.

Ownership and founder research is limited to business-relevant public information such as official pages, public filings, professional profiles, press, patents, trademarks, and public code/package metadata. Omit irrelevant personal details even if they appear in public records.

## Repository layout

```text
skills/deep-research-suite/
├── SKILL.md
└── scripts/
    └── research
```

## Attribution

This repository combines and adapts two agent workflows: a Deep Research Pro-style no-API synthesis workflow and a Deep OSINT Research-style lawful public-source investigation workflow. The helper script is adapted from the MIT-licensed Deep Research Pro helper by AstralSage; see [`NOTICE.md`](NOTICE.md).

## License

MIT. See [`LICENSE`](LICENSE).
