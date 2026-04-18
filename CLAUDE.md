# Job Application Agent — Rules & Configuration

## Target Roles
- Data Scientist
- Data Analyst
- Senior Data Analyst
- Data Science Consultant
- Financial Analyst
- Analytics Engineer
- Business Intelligence Analyst
- Machine Learning Consultant
- Quantitative Analyst

## Skip Titles Containing
Principal, Staff, Director, VP, Vice President, Head of, Junior, Entry Level, Intern

## Match Threshold
Apply only when score >= 80 out of 100.
Scoring: title(30) + skills(40) + experience(20) + domain(10)

## Job Age Filter
Rolling 24-hour window. Poll every 30 minutes. Apply immediately to new postings.

## Salary Filter
- Posted range: skip if midpoint < $amount
- No salary listed: apply anyway
- Form fields: enter Salary Range, single field: "salary"


## Work Authorization
"Legally authorized to work in US?" → YES

## Resume Tailoring Rules
1. Reorder bullets — most relevant experience first for the specific JD
2. Add 2-4 missing keywords from JD into existing bullets where they fit
3. Never change more than 30% of any bullet wording
4. Never add skills or jobs not in resume_base.md
5. Never invent anything

## Company Tiers
Tier 1: Google, Microsoft, Amazon, Apple, Meta, Nvidia, Salesforce, Adobe,
Netflix, Uber, Lyft, Airbnb, LinkedIn, Stripe, Databricks, Snowflake,
JPMorgan, Goldman Sachs, Morgan Stanley, Capital One, Bank of America
Tier 2: Any company 500+ employees not triggering H1B skip filter
Skip: Companies under 50 employees

## Deduplication
Never apply to same URL twice. Track in wiki/applied_urls.txt
