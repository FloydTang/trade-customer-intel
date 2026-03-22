# Trade Customer Intel

Open-source Codex skill for bilingual, evidence-backed public-web customer intelligence reports for foreign-trade leads.

It takes sparse lead data such as a company name, contact name, email, or website and turns it into a structured Markdown and JSON report for sales verification, account research, and conservative outreach preparation.

## What It Does

- Resolves company and contact identity from public-web evidence
- Searches official sites, LinkedIn, Facebook, Instagram, X/Twitter, YouTube, and general web results
- Produces a CRM-friendly bilingual report
- Scores lead risk as `Low`, `Medium`, or `High`
- Generates a conservative outreach persona card and English outreach pack when evidence is strong enough

## Repository Structure

```text
.
├── SKILL.md
├── agents/
│   └── openai.yaml
├── references/
│   ├── report-template.md
│   └── source-playbook.md
├── scripts/
│   └── build_customer_intel_report.py
├── examples/
│   └── sample-input.json
└── .github/
```

## Quick Start

### 1. Prepare input

Create a JSON file like this:

```json
{
  "company_name": "Acme Industrial",
  "person_name": "Jane Smith",
  "email": "jane@acme-industrial.com",
  "company_website": "",
  "country_or_market": "United States",
  "notes": ""
}
```

### 2. Run the report builder

```bash
python3 ./scripts/build_customer_intel_report.py \
  --input-json ./examples/sample-input.json \
  --markdown-out /tmp/customer-intel.md \
  --json-out /tmp/customer-intel.json
```

Or pipe JSON directly:

```bash
cat <<'EOF' | python3 ./scripts/build_customer_intel_report.py
{
  "company_name": "Acme Industrial",
  "person_name": "Jane Smith",
  "email": "jane@acme-industrial.com",
  "country_or_market": "United States"
}
EOF
```

## Search Behavior

- If `tvly` is installed, the script uses it first for search.
- Otherwise it falls back to DuckDuckGo HTML search.
- Page text snapshots use `r.jina.ai` when available.
- When evidence is sparse, the script still generates a report and explicitly lowers confidence.

## Output

The generated report follows the structure in [references/report-template.md](./references/report-template.md), including:

- Executive summary in Chinese and English
- Identity snapshot
- Company profile
- Digital footprint table
- Interest and topic signals
- Sales angles
- Outreach persona card
- Personalized outreach pack
- Risk rating
- Evidence log

## Design Principles

- Public web only
- Conservative entity matching
- Conservative risk scoring
- No private-data claims
- No invented personalization

## Using It As a Skill

This repository is structured as a Codex skill. The main skill definition lives in [SKILL.md](./SKILL.md), and the agent-facing metadata lives in [agents/openai.yaml](./agents/openai.yaml).

## License

Released under the MIT License. See [LICENSE](./LICENSE).

## Attribution

Created and maintained by 半斤九两科技.
