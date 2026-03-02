# Prospecting Queries — Repeatable Target Finding

**Purpose:** Reusable queries to find Stage 2 niche vertical companies. Run these monthly to refresh the pipeline.

**API:** TheirStack v1 Jobs Search (POST `https://api.theirstack.com/v1/jobs/search`)
**Auth:** Bearer token via `$THEIRSTACK_API_KEY` env var
**Credits:** Free tier = 200/month. 1 credit = 1 record returned. `include_total_results: true` gives count for free.
**Limit:** Max 25 records per query on free tier.

---

## Quick-Start Prompt

Paste this into a fresh Claude Code session to re-run prospecting:

> I want to refresh my Thresh prospect pipeline. Read `outbound/prospecting-queries.md` for the API queries and `thresh.md` for the ICP framework. Run the 3 core TheirStack queries below, scan results for niche vertical companies (construction, food safety, climate, insurance, professional services, vertical SaaS), and update `outbound/pipeline.md` with any new finds. My TheirStack API key is in `$THEIRSTACK_API_KEY`. I have ~150 credits remaining (check before burning more than 50).

---

## Core Queries

### Query 1: VP/Head/Director of Sales at small companies (PRIMARY SIGNAL)

Finds the buyer persona directly. This person just arrived and discovered there's no intelligence layer.

```bash
curl -s -X POST "https://api.theirstack.com/v1/jobs/search" \
  -H "Authorization: Bearer $THEIRSTACK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "job_title_or": ["VP Sales", "Head of Sales", "VP of Sales", "Director of Sales"],
    "job_country_code_or": ["US"],
    "min_employee_count": 10,
    "max_employee_count": 200,
    "posted_at_gte": "YYYY-MM-DD",
    "include_total_results": true,
    "limit": 10
  }'
```

**Set `posted_at_gte` to 30 days ago.** Format: `2026-01-26` for a Feb 25 run.

**What to look for in results:**
- Company industry NOT in: Hospitality, Staffing, Retail, Real Estate (unless proptech)
- Employee count 10-75 = strongest signal (first sales leader)
- Recent funding round = bonus signal
- Company description mentions a niche vertical

**2026-02-25 baseline:** 778 jobs at 647 companies

---

### Query 2: First SDR/BDR at tiny companies (OUTBOUND INTENT SIGNAL)

Finds companies that just committed to outbound but have zero infrastructure.

```bash
curl -s -X POST "https://api.theirstack.com/v1/jobs/search" \
  -H "Authorization: Bearer $THEIRSTACK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "job_title_or": ["SDR", "BDR", "Sales Development Representative", "Business Development Representative"],
    "job_country_code_or": ["US"],
    "min_employee_count": 10,
    "max_employee_count": 75,
    "posted_at_gte": "YYYY-MM-DD",
    "include_total_results": true,
    "limit": 10
  }'
```

**What to look for:** Same filtering as Query 1. Extra bonus: companies with <25 employees posting SDR = almost certainly their first outbound hire.

**2026-02-25 baseline:** 893 jobs at 755 companies

---

### Query 3: RevOps / Sales Ops / GTM Engineer at small companies (INFRASTRUCTURE SIGNAL)

Finds companies building ops for the first time. The "GTM Engineer" title catches companies at the exact inflection point.

```bash
curl -s -X POST "https://api.theirstack.com/v1/jobs/search" \
  -H "Authorization: Bearer $THEIRSTACK_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "job_title_or": ["RevOps", "Revenue Operations", "Sales Operations Manager", "GTM Engineer"],
    "job_country_code_or": ["US"],
    "min_employee_count": 10,
    "max_employee_count": 150,
    "posted_at_gte": "YYYY-MM-DD",
    "include_total_results": true,
    "limit": 10
  }'
```

**2026-02-25 baseline:** 311 jobs at 258 companies

---

## Free Tier Limitations

TheirStack free tier does NOT support:
- `company_industry_or` (filter by company industry)
- `company_keyword_or` (filter by company keywords)
- `job_description_pattern_and` (filter by JD content)
- `company_description_pattern_or` (filter by company description)

**Workaround:** Pull broad results (10-25 per query) and manually scan for niche vertical companies. The `company_object.industry` and `company_object.long_description` fields in the response tell you what the company does.

**Alternative workaround:** Search for vertical-specific job titles (e.g., `"construction sales"`, `"food safety sales"`). This works but returns actual industry companies hiring salespeople, not SaaS companies selling to that industry. Filter carefully.

---

## Result Parsing Script

Pipe any query through this to get a clean summary:

```bash
| python3 -c "
import json, sys
data = json.load(sys.stdin)
print(f\"Total: {data['metadata']['total_results']} jobs at {data['metadata']['total_companies']} companies\n\")
for job in data['data']:
    co = job.get('company_object', {})
    print(f\"Company: {job['company']} ({co.get('employee_count', '?')} employees)\")
    print(f\"  Domain: {job.get('company_domain', 'N/A')} | Industry: {co.get('industry', 'N/A')}\")
    print(f\"  Role: {job['job_title']}\")
    print(f\"  Posted: {job['date_posted']} | Location: {job.get('location', 'N/A')}\")
    print(f\"  Revenue: {co.get('annual_revenue_usd_readable', 'N/A')}\")
    print(f\"  Funding: {co.get('total_funding_usd', 'N/A')} | Last round: {co.get('last_funding_round_date', 'N/A')} ({co.get('last_funding_round_amount_readable', 'N/A')})\")
    desc = co.get('long_description', '')[:180]
    print(f\"  About: {desc}...\")
    print()
"
```

---

## Credit Budget

| Action | Credits |
|--------|---------|
| Query with `limit: 1` (just get total count) | 1 |
| Query with `limit: 10` | 10 |
| Query with `limit: 25` (max free tier) | 25 |
| Recommended per monthly refresh: 3 core queries x 10 | 30 |
| Extra vertical-specific queries (2-3) | 15-30 |
| **Total per refresh** | **~45-60** |

At 200 credits/month, you can do 3 full refreshes + have credits left for run-specific Step 6 validation.
