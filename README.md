# Autonomous Job Application Agent

An end-to-end autonomous AI pipeline that applies to data science jobs autonomously — scraping, scoring, tailoring resumes, submitting applications, and tracking everything in a dashboard.

## What it does

1. Scrapes LinkedIn, Google Careers, Microsoft, Amazon, Apple, and Workday portals every 30 minutes
2. Scores each job against my resume (0–100) across title fit, skills overlap, experience, and domain
3. Skips jobs that doesnt meet some of the criterias that I have, or are older than 24 hours
4. Generates a tailored PDF resume per job using the Anthropic Claude API
5. Fills and submits application forms autonomously via Playwright
6. Logs every action to a persistent wiki and displays it on a Streamlit dashboard

## Tech Stack

| Layer | Tools |
|---|---|
| Scraping | Playwright, browser-use |
| Matching | Python NLP, keyword scoring |
| Resume tailoring | Anthropic Claude API (claude-sonnet-4-5) |
| Form filling | Playwright (raw, session-aware) |
| Dashboard | Streamlit |
| PDF generation | ReportLab |
| Scheduling | cron (9 AM EST daily) |
| Memory | LLM Wiki pattern (markdown files) |

## Folder Structure

autonomous-job-agent/
├── CLAUDE.md                    # Agent rules and configuration
├── schema.md                    # Data contracts for all wiki files
├── agent/
│   ├── scraper.py               # Multi-portal job scraper (30-min polling loop)
│   ├── matcher.py               # Resume-JD scoring engine
│   ├── tailor.py                # Claude-powered resume tailoring + PDF export
│   ├── filler.py                # Playwright form submission (session-aware)
│   └── logger.py                # Appends results to wiki/log.md
├── dashboard/
│   └── app.py                   # Streamlit dashboard (real-time, auto-refresh)
├── requirements.txt
├── run_agent.sh                 # Main orchestrator
└── setup_cron.sh                # Installs daily cron job



## Matching Score Rubric

| Dimension | Weight |
|---|---|
| Title alignment | 30 pts |
| Skills overlap | 40 pts |
| Years of experience | 20 pts |
| Domain fit | 10 pts |
| **Minimum to apply** | **80 / 100** |

## Filters Applied Before Every Application

- current Criteria - specific to individual
- Salary: skips if posted range midpoint < $dollars
- Recency: skips if job posted more than 24 hours ago
- Seniority: skips Principal, Staff, Director, VP, Head of, Junior, Intern titles
- Deduplication: never applies to the same URL twice

## Resume Tailoring Rules

The agent tailors — it does not fabricate. Rules enforced via CLAUDE.md:
- Reorders bullet points so most relevant experience appears first
- Adds 2–4 missing keywords from the JD into existing bullets where they genuinely fit
- Never changes more than 30% of any bullet wording
- Never adds skills, jobs, or dates not in the base resume
- Saves one PDF per application named `company_role_date.pdf`

## Dashboard

Run `streamlit run dashboard/app.py` → open `http://localhost:8501`

Shows: applied today, total submitted, skip breakdown, status chart, filterable table with links.

## Setup

```bash
git clone https://github.com/name/autonomous-job-agent
cd autonomous-job-agent
python3.12 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
playwright install chromium
cp .env.example .env
# Add ANTHROPIC_API_KEY to .env
./run_agent.sh --once
```

## Cost Estimate

| Action | Cost |
|---|---|
| Scraping + matching | ~$0.00 (no API calls) |
| Resume tailoring per job | ~$0.10–0.30 |
| Full nightly run (5–10 applications) | ~$1–3 |

## Built With

Python · Playwright · Anthropic Claude API · Streamlit · ReportLab · Shell · cron
<img width="1707" height="870" alt="image" src="https://github.com/user-attachments/assets/6cf1dee5-c83b-4dcd-84ab-01e6e78facb9" />

