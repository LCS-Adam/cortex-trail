# Snowflake Cortex AI Cost Calculator



**Purpose:** Reference implementation for Cortex cost monitoring and forecasting  


---

**Monitor Cortex usage and forecast future costs with confidence.**

A professional toolkit for tracking Snowflake Cortex service consumption and generating accurate cost projections. Perfect for engineering teams during scoping exercises and for clients managing their AI workload budgets.

---

## 👋 First Time Here?

**Follow these steps in order:**

1. **Deploy Everything (Recommended):** [`deploy_all.sql`](deploy_all.sql) - Copy/paste into Snowsight → Click "Run All" (~2 min)
2. **Access Calculator:** Snowsight → Projects → Streamlit → CORTEX_COST_CALCULATOR
3. **Export Metrics (Optional):** [`sql/02_utilities/export_metrics.sql`](sql/02_utilities/export_metrics.sql) - For two-account workflow

**Total setup time: ~2 minutes**

---

**Alternative: Step-by-step deployment**

1. **Deploy Monitoring:** [`sql/01_deployment/deploy_cortex_monitoring.sql`](sql/01_deployment/deploy_cortex_monitoring.sql) (~1 min)
2. **View Results:** Query views or continue to Streamlit deployment
3. **Deploy Calculator:** See [Streamlit deployment section ↓](#deploy-streamlit-calculator) (~2 min)

**Total setup time: ~3-5 minutes**

---

**Want more guidance?**

- **Understand the architecture first:** [`docs/01-GETTING_STARTED.md`](docs/01-GETTING_STARTED.md) - Concepts and design (~5 min)
- **Need detailed walkthrough:** [`docs/02-DEPLOYMENT_WALKTHROUGH.md`](docs/02-DEPLOYMENT_WALKTHROUGH.md) - Step-by-step with validation (~15 min)
- **Troubleshooting help:** [`docs/03-TROUBLESHOOTING.md`](docs/03-TROUBLESHOOTING.md) - Common issues and solutions

---

## 🚀 Quick Start

Deploy directly in your Snowflake account for ongoing cost monitoring and forecasting.

### Installation Time
- **Initial Setup:** < 5 minutes
- **Per-client Analysis:** 5-10 minutes

### What You Get
- ✅ **16 monitoring views** tracking all Cortex services
- ✅ **Query-level cost tracking** - identify expensive queries
- ✅ **Document processing analysis** - compare PARSE_DOCUMENT vs Document AI
- ✅ **Fine-tuning ROI tracking** - separate training vs inference costs
- ✅ **Cortex Search optimization** - hourly cost tracking to find idle periods
- ✅ **Latest pricing** - Current rates for Cortex LLM models (15+ available)
- ✅ **Serverless task** - automatic daily snapshots, no warehouse needed
- ✅ **Snapshot table** - 4-5x faster queries
- ✅ **Simplified Cost per User Calculator** for quick scoping
- ✅ **30-day rolling totals** for accurate estimates
- ✅ **Smart data fallback** - works immediately after deployment
- ✅ **Historical trend analysis** with week-over-week growth
- ✅ **Export-ready** credit estimates for proposals
- ✅ **Zero disruption** to production workloads

---

## Quick Start Guides

**Self-Service Deployment**

Deploy both monitoring and calculator in your own Snowflake account to track and forecast Cortex costs continuously.

#### Step 1: Deploy Monitoring

```sql
-- Run in your Snowflake account
@sql/01_deployment/deploy_cortex_monitoring.sql
```

**Requirements:**
- `ACCOUNTADMIN` role OR role with `IMPORTED PRIVILEGES` on `SNOWFLAKE` database
- Active warehouse

**What gets created:**
- Database: `SNOWFLAKE_EXAMPLE`
- Schema: `CORTEX_USAGE`
- 9 views tracking Cortex usage

#### Step 2: Deploy Calculator

**See detailed instructions:** [Deploy Streamlit Calculator](#deploy-streamlit-calculator) (2-3 minutes)

#### Step 3: Access Your Calculator

1. Open the Streamlit app
2. Select **"Query Views (Same Account)"** as data source
3. Set lookback period (default: 30 days)
4. View historical analysis and projections

**The calculator automatically queries your monitoring views in real-time.**

---

## Key Features

### Serverless Task (No Warehouse Required!)
- **Automatic daily snapshots** at 3:00 AM
- **No configuration** - Snowflake manages compute
- **Low cost**: ~$0.30-1.50/month (vs dedicated warehouse)
- Captures data to `CORTEX_USAGE_SNAPSHOTS` table

### Snapshot Table for Speed
- **4-5x faster queries** compared to ACCOUNT_USAGE
- **Pre-calculated metrics** (no compute-on-query)
- **Historical tracking** beyond 90-day rolling window
- `V_CORTEX_USAGE_HISTORY` view includes week-over-week trends

### Simplified Cost per User Calculator
- Moved to **top of Cost Projections tab**
- **Historical reference table** shows actual usage
- **Simple persona configuration**: name, user count, requests/day
- **Instant estimates** for per-persona and total costs

### Smart Data Fetching
- **Automatic fallback**: Tries snapshot table first, falls back to live view
- **Works immediately** after deployment (doesn't wait for 3 AM)
- **Best of both worlds**: Fast when available, always functional

### Streamlined Experience
- **Removed** Scenario Comparison tab
- **Clean documentation** in `docs/` folder (user-facing only)
- **Streamlined codebase** for easy understanding
- **Essential files only** for quick learning

---

## What This Tool Does

### Cortex Services Tracked

This tool monitors all Snowflake Cortex services:

| Service | Description | Documentation |
|---------|-------------|---------------|
| **Cortex Analyst** | Natural language analytics with semantic models | [Docs](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-analyst) |
| **Cortex Search** | Vector and hybrid search services | [Docs](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-search) |
| **Cortex Functions** | LLM functions (AI_COMPLETE, AI_TRANSLATE, AI_CLASSIFY, AI_EMBED, AI_TRANSCRIBE, AI_SENTIMENT, etc.) - Note: AI_TRANSLATE supersedes TRANSLATE with 20% cost savings | [Docs](https://docs.snowflake.com/en/user-guide/snowflake-cortex/llm-functions) |
| **Document AI** | OCR and document processing | [Docs](https://docs.snowflake.com/en/user-guide/snowflake-cortex/document-ai) |

### Data Captured

- **Usage metrics:** Requests, tokens processed, messages sent, pages processed
- **Credit consumption:** Actual credit usage from `ACCOUNT_USAGE` views
- **User activity:** User-level tracking where available
- **Time series:** Daily, hourly, or per-request granularity depending on service

### Cost Calculator Features

#### 📈 Historical Analysis
- Interactive time series charts
- Service-by-service breakdown
- Cost trends over time
- User activity metrics

#### 🔮 Cost Projections
- Simplified Cost per User Calculator at top
  - Historical reference table from actual usage data
  - Configure personas (name, user count, requests/day)
  - Instant per-persona and total cost estimates
- Multi-scenario growth projections
- 3, 6, 12, or 24-month forecasts
- Variance ranges for confidence intervals
- Custom growth rate modeling

#### 📋 Summary Reports
- Pre-formatted credit estimates
- Service-level cost breakdown
- Monthly projection tables
- Ready for proposals and budgets

---

## Prerequisites

### Required

✅ **Snowflake account** with Cortex usage (ideally 7-14 days of history)

✅ **One of the following roles:**
- `ACCOUNTADMIN` role, OR
- Role with `IMPORTED PRIVILEGES` on `SNOWFLAKE` database

✅ **Active warehouse** for running queries

### Optional (For Streamlit App)

✅ Privileges to create Streamlit apps in Snowflake

### Verify Access

Test that you can access `ACCOUNT_USAGE`:

```sql
-- This should return rows, not an error
SELECT COUNT(*) 
FROM SNOWFLAKE.ACCOUNT_USAGE.METERING_DAILY_HISTORY 
WHERE usage_date >= DATEADD('day', -7, CURRENT_DATE());
```

If you get "Object does not exist", grant privileges:

```sql
USE ROLE ACCOUNTADMIN;
GRANT IMPORTED PRIVILEGES ON DATABASE SNOWFLAKE TO ROLE <YOUR_ROLE>;
```

**Reference:** [Snowflake ACCOUNT_USAGE documentation](https://docs.snowflake.com/en/sql-reference/account-usage)

---

## Deployment Guide

### Deploy Monitoring Views

```sql
-- Run in Snowflake UI, SnowSQL, or any SQL client
@sql/01_deployment/deploy_cortex_monitoring.sql
```

**What happens:**
1. Creates database `SNOWFLAKE_EXAMPLE` (if not exists)
2. Creates schema `CORTEX_USAGE` (if not exists)
3. Creates 9 views querying `SNOWFLAKE.ACCOUNT_USAGE`
4. Runs validation queries to verify deployment

**Deployment validates automatically:**
- Check for "✓ SUCCESS" messages
- Verify row counts from each view
- Review any warnings about empty results

**If deployment fails:**
- Verify you have `IMPORTED PRIVILEGES` on `SNOWFLAKE` database
- Check that warehouse is running
- Review error messages for specific issues
- See [Troubleshooting](#troubleshooting) section

### The 9 Monitoring Views

| View | Purpose | Granularity | User Tracking |
|------|---------|-------------|---------------|
| `V_CORTEX_ANALYST_DETAIL` | Analyst usage | Per request | ✅ Yes (username) |
| `V_CORTEX_SEARCH_DETAIL` | Search daily usage | Daily aggregate | ❌ No |
| `V_CORTEX_SEARCH_SERVING_DETAIL` | Search serving | Hourly detail | ❌ No |
| `V_CORTEX_FUNCTIONS_DETAIL` | Functions by model | Hourly aggregate | ❌ No |
| `V_CORTEX_FUNCTIONS_QUERY_DETAIL` | Functions by query | Per query | ✅ Yes (query_id) |
| `V_DOCUMENT_DETAIL` | Document processing | Per request | ✅ Yes (query_id) |
| `V_CORTEX_DAILY_SUMMARY` | **Rollup across all services** | Daily | Derived |
| `V_CORTEX_COST_EXPORT` | **Pre-formatted for calculator** | Daily | Derived |
| `V_METERING_SERVICES` | High-level validation | Daily | ❌ No |

**Primary views for analysis:** `V_CORTEX_DAILY_SUMMARY` and `V_CORTEX_COST_EXPORT`

### Deploy Streamlit Calculator

#### Method 1: Snowsight UI (Recommended)

1. Log into [Snowsight](https://app.snowflake.com)
2. Navigate: **Projects** > **Streamlit** > **Apps** > **"+ Streamlit App"**
3. Configure app:
   - **App name:** `CORTEX_COST_CALCULATOR`
   - **Location:** `SNOWFLAKE_EXAMPLE.CORTEX_USAGE` (or your preferred database/schema)
   - **Warehouse:** Select appropriate warehouse (SMALL is fine)
4. **Code editor:** Copy entire contents of `streamlit_app.py`
5. **Packages:** Copy contents of `environment.yml` to package section
6. Click **"Create"**

**Reference:** [Streamlit in Snowflake documentation](https://docs.snowflake.com/en/developer-guide/streamlit/about-streamlit)

#### Method 2: SnowSQL CLI (Advanced)

```sql
-- 1. Create stage
CREATE STAGE IF NOT EXISTS SNOWFLAKE_EXAMPLE.CORTEX_USAGE.STREAMLIT_STAGE;

-- 2. Upload files (run in terminal)
-- snow stage put file://streamlit_app.py @SNOWFLAKE_EXAMPLE.CORTEX_USAGE.STREAMLIT_STAGE
-- snow stage put file://environment.yml @SNOWFLAKE_EXAMPLE.CORTEX_USAGE.STREAMLIT_STAGE

-- 3. Create Streamlit app
CREATE STREAMLIT SNOWFLAKE_EXAMPLE.CORTEX_USAGE.CORTEX_COST_CALCULATOR
  ROOT_LOCATION = '@SNOWFLAKE_EXAMPLE.CORTEX_USAGE.STREAMLIT_STAGE'
  MAIN_FILE = '/streamlit_app.py'
  QUERY_WAREHOUSE = 'YOUR_WAREHOUSE_NAME';

-- 4. Grant access to users
GRANT USAGE ON STREAMLIT CORTEX_COST_CALCULATOR TO ROLE <ROLE_NAME>;
```

### Grant Access to Other Users

```sql
-- Grant view access
GRANT USAGE ON DATABASE SNOWFLAKE_EXAMPLE TO ROLE <USER_ROLE>;
GRANT USAGE ON SCHEMA SNOWFLAKE_EXAMPLE.CORTEX_USAGE TO ROLE <USER_ROLE>;
GRANT SELECT ON ALL VIEWS IN SCHEMA SNOWFLAKE_EXAMPLE.CORTEX_USAGE TO ROLE <USER_ROLE>;

-- Grant Streamlit access (if deployed)
GRANT USAGE ON STREAMLIT CORTEX_COST_CALCULATOR TO ROLE <USER_ROLE>;
```

---

## Verification Checklist

After deployment, verify everything is working correctly:

- [ ] **Database created:** `SHOW DATABASES LIKE 'SNOWFLAKE_EXAMPLE'` returns 1 row
- [ ] **Schema created:** `SHOW SCHEMAS IN DATABASE SNOWFLAKE_EXAMPLE` includes CORTEX_USAGE
- [ ] **Views created:** `SHOW VIEWS IN SCHEMA SNOWFLAKE_EXAMPLE.CORTEX_USAGE` returns 16 rows
- [ ] **Table created:** `SHOW TABLES IN SCHEMA SNOWFLAKE_EXAMPLE.CORTEX_USAGE` includes CORTEX_USAGE_SNAPSHOTS
- [ ] **Task created:** `SHOW TASKS IN SCHEMA SNOWFLAKE_EXAMPLE.CORTEX_USAGE` includes TASK_DAILY_CORTEX_SNAPSHOT
- [ ] **Task running:** Task status is "started" (check with `SHOW TASKS`)
- [ ] **Views return data:** `SELECT * FROM V_CORTEX_DAILY_SUMMARY LIMIT 1` returns rows (empty if no usage yet)
- [ ] **Streamlit accessible:** App loads without errors (if deployed)
- [ ] **Charts render:** Historical analysis displays visualizations

---

## Next Steps

After successful deployment, enhance your monitoring setup:

1. **Set up alerts** - Configure resource monitors for Cortex budget tracking
2. **Grant access** - Share views/app with other users via GRANT statements (see [Deployment Guide](#deployment-guide))
3. **Customize views** - Modify lookback periods or filters to match your organization's needs
4. **Automate exports** - Schedule regular CSV exports or integrate views with your BI tools
5. **Analyze query costs** - Use `V_QUERY_COST_ANALYSIS` to identify expensive queries and optimize
6. **Monitor trends** - Check `V_CORTEX_DAILY_SUMMARY` for usage patterns and week-over-week growth

**Full documentation:** See [`docs/`](docs/) directory for detailed guides on architecture, deployment, and troubleshooting.

---

## Using the Cost Calculator

### Access the Calculator

**Snowsight:** Projects > Streamlit > Apps > CORTEX_COST_CALCULATOR

### Data Source Options

#### Option 1: Query Views (Same Account)
- Select **"Query Views (Same Account)"**
- Set lookback period (default: 30 days)
- Calculator queries monitoring views directly
- Best for ongoing monitoring in your own account

#### Option 2: Upload Customer CSV
- Select **"Upload Customer CSV"**
- Drag and drop CSV file from extraction query
- Best for engineering workflow analyzing customer data
- Supports multiple customers without loading to database

### Calculator Tabs

#### 1️⃣ Historical Analysis

**Summary Statistics:**
- Total credits consumed
- Total cost (credits × credit price)
- Date range covered
- Service breakdown

**Charts:**
- Credits consumed over time
- Cost by service type
- Daily usage patterns
- User activity metrics (where available)

**Use this to:**
- Validate data quality
- Identify usage trends
- Understand service mix
- Detect anomalies

#### 2️⃣ Cost Projections

**Projection Periods:**
- 3 months
- 6 months
- 12 months
- 24 months

**Growth Scenarios:**

| Scenario | Monthly Growth | Use Case |
|----------|----------------|----------|
| **Conservative** | 10% | Steady adoption, existing use cases |
| **Moderate** | 25% | Active expansion, new features |
| **Aggressive** | 50% | Rapid rollout, multiple teams |
| **Rapid** | 100% | Explosive growth, company-wide adoption |
| **Custom** | You define | Specific business plan |

**Charts:**
- Monthly cost projections with variance bands
- Cumulative cost over time
- Cost range visualization

**💰 Cost per User Calculator (NEW):**

Calculate estimated costs per user based on usage patterns:

**Features:**
- Define expected operations per user per service
- Adjust usage intensity multiplier
- Set active days per month
- See cost breakdown by service
- Visual pie chart of credit distribution

**Output Metrics:**
- Credits per user per month
- Cost per user per month
- Service-by-service breakdown
- Percentage distribution across services

**📊 Budget Capacity Calculator (NEW):**

Determine how many users you can support with a given budget:

**Inputs:**
- Monthly budget (USD)
- Safety buffer percentage (for unexpected spikes)
- Usage pattern from calculator above

**Output Metrics:**
- Number of users supported
- Total monthly credits consumed
- Budget utilization percentage
- Remaining budget capacity
- Detailed capacity insights table

**Calculation Methodology:**
- Based on historical usage patterns
- Uses actual credits per operation from your data
- Adjustable for different usage scenarios
- Includes safety buffer for variance
- Shows detailed breakdown of all calculations

**Use this to:**
- Forecast future spend
- Budget planning
- Capacity planning
- Proposal creation
- Per-user cost estimation
- License/user capacity planning
- Budget allocation decisions

#### 3️⃣ Scenario Comparison

**Side-by-Side Analysis:**
- Compare all scenarios at once
- Adjustable parameters per scenario
- Export comparison table

**Custom Scenario Builder:**
- User growth rate (adoption)
- Usage intensity growth (per-user consumption)
- Separate modeling for different growth drivers

**Use this to:**
- Present multiple options to stakeholders
- Model best/worst/likely cases
- Understand sensitivity to growth assumptions

#### 4️⃣ Summary Report

**Credit Estimate Export:**
- Service-by-service breakdown
- Daily average credits
- Monthly projection
- Cost estimates

**Download as CSV:**
- Pre-formatted for sales/pricing teams
- Ready to incorporate into proposals
- Includes methodology notes

**Use this to:**
- Share estimates with sales team
- Create proposals
- Budget documentation
- Stakeholder presentations

### Configuration Options

**Sidebar Settings:**

- **Credit Cost ($/credit):** Adjust based on your Snowflake contract
  - Default: $3.00
  - Verify with your Snowflake account team
  - Varies by edition, region, and contract terms

- **Projection Variance (%):** Confidence interval width
  - Default: 10%
  - Increase for higher uncertainty
  - Decrease for more predictable workloads

- **Refresh Data:** Clear cache and reload

---

## Understanding Your Results

### Key Metrics Explained

**Credits Consumed:**
- Snowflake compute credits used by Cortex services
- Source: `ACCOUNT_USAGE` views (authoritative billing data)
- Matches what appears on your Snowflake bill

**Cost:**
- Credits × Credit Price (configured in calculator)
- **Important:** Credit prices vary by contract
- Always verify pricing with Snowflake account team

**Credits Per User:**
- Average daily credits per active user
- Only calculated when user tracking available
- Useful for per-seat cost modeling

**Credits Per Operation:**
- Average credits per request/token/page processed
- Varies significantly by service and workload
- Use for capacity planning

### Projection Methodology

**How projections work:**
1. **Baseline:** Average daily credit usage from historical data
2. **Growth rate:** Compound monthly growth applied to baseline
3. **Variance:** ±X% range around projection (configurable)
4. **Cumulative:** Sum of all months in projection period

**Formula:**
```
Monthly Credits = Baseline × (1 + Growth Rate) ^ Month Number
Monthly Cost = Monthly Credits × Credit Price
```

**Important Notes:**
- Projections assume compound growth (exponential)
- Not guarantees - use as directional estimates
- Update monthly as actual usage accumulates
- Consider seasonality and adoption curves

### Data Accuracy

**What's accurate:**
- ✅ Historical credit consumption (from `ACCOUNT_USAGE`)
- ✅ Service breakdown (from detailed usage views)
- ✅ Date ranges and time series (actual data)

**What's estimated:**
- ⚠️ Future growth rates (user-defined assumptions)
- ⚠️ User adoption patterns (may vary significantly)
- ⚠️ Credit pricing (depends on contract terms)

**Best practice:**
- Present projections with variance ranges
- Document assumptions clearly
- Update forecasts as actuals come in
- Use conservative scenarios for budgeting

### Data Latency

⚠️ **ACCOUNT_USAGE has 45 minutes to 3 hours latency**

- Recent usage may not appear immediately
- Wait 3+ hours after activity before expecting data
- Not suitable for real-time monitoring
- Designed for historical analysis and planning

**Reference:** [Snowflake ACCOUNT_USAGE latency documentation](https://docs.snowflake.com/en/sql-reference/account-usage#label-account-usage-views)

---

## FAQ

### General Questions

**Q: Is this safe to deploy in production?**  
A: Yes. The solution is read-only, creates isolated views, and has zero impact on existing workloads. It only queries `ACCOUNT_USAGE` views which are designed for this purpose.

**Q: Will this impact our Snowflake bill?**  
A: Minimal impact. View queries use your existing warehouse and consume trivial credits. Typical cost: < $1/month.

**Q: Can we customize the views?**  
A: Yes. All SQL is provided and can be modified. Add filters, change date ranges, or create custom aggregations.

**Q: How do we remove everything?**  
A: Run `sql/99_cleanup/cleanup_all.sql`. Complete removal in seconds. See [Cleanup](#cleanup--removal) section.

### Data Questions

**Q: Why are my views empty?**  
A: Three common reasons:
1. No Cortex usage in the lookback period (check with `SELECT * FROM SNOWFLAKE.ACCOUNT_USAGE.METERING_DAILY_HISTORY WHERE service_type = 'AI_SERVICES'`)
2. Data latency (wait 3 hours after usage)
3. Lookback period too short (extend to 90 or 180 days)

**Q: Some services show no user counts. Why?**  
A: This is expected. Some `ACCOUNT_USAGE` views (like Cortex Search hourly aggregates) don't include user-level detail. User tracking is available for Cortex Analyst, Document AI, and Functions (query-level).

**Q: Can I export data to Excel/Tableau/PowerBI?**  
A: Yes. Query any view and use Snowflake's download feature to export as CSV. Import into your tool of choice.

**Q: How far back can I query historical data?**  
A: Limited by `ACCOUNT_USAGE` retention, typically 1 year. Adjust lookback period in queries as needed.

### Calculator Questions

**Q: Can multiple engineers/teams use the same calculator?**  
A: Yes. Each uploads their customer's CSV independently. No data is stored between sessions.

**Q: What credit price should I use?**  
A: Verify with the customer's Snowflake account team. Credit prices vary by:
- Snowflake edition (Standard, Enterprise, Business Critical)
- Cloud provider and region (AWS, Azure, GCP)
- Contract terms and commitments
- On-demand vs capacity pricing

**Reference:** [Snowflake pricing documentation](https://www.snowflake.com/pricing/)

**Q: How accurate are the projections?**  
A: Projections are directional estimates based on your growth assumptions. They become more accurate with:
- Longer historical periods (14+ days recommended)
- Stable usage patterns
- Regular updates as actuals come in

Always present projections with variance ranges.

**Q: Can customers use this themselves?**  
A: Absolutely. Customers can deploy both monitoring and calculator in their own account for self-service cost management.

### Technical Questions

**Q: What Snowflake privileges are required?**  
A: Minimum requirements:
- `IMPORTED PRIVILEGES` on `SNOWFLAKE` database (for `ACCOUNT_USAGE` access)
- `CREATE DATABASE` on account (for deployment)
- `CREATE SCHEMA` on database
- `CREATE VIEW` on schema

Or simply use `ACCOUNTADMIN` role.

**Q: Can this run on Snowflake Marketplace?**  
A: Not currently, but views can be shared via [Secure Data Sharing](https://docs.snowflake.com/en/user-guide/data-sharing-intro).

**Q: What Python packages are required?**  
A: All listed in `environment.yml`:
- streamlit (pre-installed in Streamlit in Snowflake)
- snowflake-snowpark-python (pre-installed)
- pandas, numpy (commonly available)
- plotly (for visualizations)

**Q: Can I schedule the calculator to email reports?**  
A: Not directly. The Streamlit app is interactive. For scheduled reports, query the views directly and use [Snowflake tasks](https://docs.snowflake.com/en/user-guide/tasks-intro) to email results.

---

## Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| "Object does not exist" on ACCOUNT_USAGE | `GRANT IMPORTED PRIVILEGES ON DATABASE SNOWFLAKE TO ROLE <role>` |
| Views return no data | Wait 3 hours for ACCOUNT_USAGE latency; verify Cortex usage exists |
| Streamlit won't load | Check warehouse is running; verify app location matches deployment |
| Permission denied errors | Use ACCOUNTADMIN or grant required privileges |
| CSV upload fails | Verify CSV came from `export_metrics.sql` |
| Charts not displaying | Clear browser cache; verify plotly installed |

### Detailed Troubleshooting

See `docs/03-TROUBLESHOOTING.md` for comprehensive troubleshooting guide including:
- Deployment issues
- Permission errors
- Data quality problems
- Calculator issues
- Performance optimization
- Debug logging
- Support bundle generation

### Getting Help

**For support:**
- **Documentation:** [docs.snowflake.com](https://docs.snowflake.com)

---

## Cleanup & Removal

### Complete Removal

```sql
-- Run cleanup script
sql/99_cleanup/cleanup_all.sql
```

The script provides three removal options:

**Option 1: Drop views only** (keeps database and schema)
```sql
DROP VIEW IF EXISTS V_CORTEX_COST_EXPORT;
DROP VIEW IF EXISTS V_CORTEX_DAILY_SUMMARY;
-- ... (drops all 9 views)
```

**Option 2: Drop schema** (removes views and schema)
```sql
DROP SCHEMA IF EXISTS SNOWFLAKE_EXAMPLE.CORTEX_USAGE CASCADE;
```

**Option 3: Drop database** (complete removal)
```sql
DROP DATABASE IF EXISTS SNOWFLAKE_EXAMPLE CASCADE;
```

**Remove Streamlit app:**
```sql
DROP STREAMLIT IF EXISTS CORTEX_COST_CALCULATOR;
```

### Safety

✅ **Cleanup is completely safe:**
- Only affects monitoring objects (views, schema, database)
- Never touches source data in `ACCOUNT_USAGE`
- Never touches client data or other databases
- Can re-deploy instantly if needed

---

## Reference Documentation

### Snowflake Cortex

- [Cortex Overview](https://docs.snowflake.com/en/user-guide/snowflake-cortex)
- [Cortex Analyst](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-analyst)
- [Cortex Search](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-search)
- [Cortex LLM Functions](https://docs.snowflake.com/en/user-guide/snowflake-cortex/llm-functions)
- [Document AI](https://docs.snowflake.com/en/user-guide/snowflake-cortex/document-ai)

### ACCOUNT_USAGE Views

- [ACCOUNT_USAGE Overview](https://docs.snowflake.com/en/sql-reference/account-usage)
- [CORTEX_ANALYST_USAGE_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/cortex_analyst_usage_history)
- [CORTEX_SEARCH_DAILY_USAGE_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/cortex_search_daily_usage_history)
- [CORTEX_FUNCTIONS_USAGE_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/cortex_functions_usage_history)
- [DOCUMENT_AI_USAGE_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/document_ai_usage_history)
- [METERING_DAILY_HISTORY](https://docs.snowflake.com/en/sql-reference/account-usage/metering_daily_history)

### Streamlit in Snowflake

- [Streamlit in Snowflake Documentation](https://docs.snowflake.com/en/developer-guide/streamlit/about-streamlit)
- [Create Streamlit Apps](https://docs.snowflake.com/en/developer-guide/streamlit/create-streamlit-ui)
- [Streamlit Packages](https://docs.snowflake.com/en/developer-guide/streamlit/package-dependencies)

### Snowflake Pricing

- [Snowflake Pricing Overview](https://www.snowflake.com/pricing/)
- [Understanding Compute Costs](https://docs.snowflake.com/en/user-guide/cost-understanding-compute)
- [Resource Monitors](https://docs.snowflake.com/en/user-guide/resource-monitors)

---

## Project Structure

```
AI_Scoping/
├── README.md                          # This file - complete guide
├── images/                            # Screenshots and visuals
│   ├── streamlit_screenshot.png       # Streamlit app preview
│   └── README.md                      # Screenshot instructions
│
├── docs/                              # User-facing documentation
│   ├── 01-GETTING_STARTED.md          # Getting started guide
│   ├── 02-DEPLOYMENT_WALKTHROUGH.md   # Deployment walkthrough
│   └── 03-TROUBLESHOOTING.md          # Issue resolution
│
├── deploy_all.sql                              # ⚡ ONE-COMMAND: Deploy everything (Git-integrated, project root)
│
├── sql/
│   ├── 01_deployment/
│   │   └── deploy_cortex_monitoring.sql            # Deploy monitoring only (16 views + task + table)
│   ├── 02_utilities/
│   │   └── export_metrics.sql                      # Extract data for SE workflow
│   └── 99_cleanup/
│       └── cleanup_all.sql                         # Remove all objects (complete cleanup)
│
└── streamlit/cortex_cost_calculator/
    ├── streamlit_app.py               # Full-featured calculator with v2.0 features
    └── environment.yml                # Package dependencies
```

**Essential files** organized for clarity and ease of use.

---

## Best Practices

### For Deployment

1. ✅ **Test in sandbox first** - Deploy to dev/test account before production
2. ✅ **Verify permissions** - Check `ACCOUNT_USAGE` access before deployment
3. ✅ **Document customizations** - Note any changes to lookback periods or naming
4. ✅ **Grant minimal access** - Only give SELECT on views, not entire database

### For Cost Estimation

1. ✅ **Include variance ranges** - Never present single-point estimates
2. ✅ **Update monthly** - Refresh projections as actual usage accumulates
3. ✅ **Validate pricing** - Confirm credit costs with Snowflake account team
4. ✅ **Show assumptions** - Document growth rates and methodology clearly
5. ✅ **Track accuracy** - Compare projections to actuals over time

### For Stakeholder Presentations

1. ✅ **Lead with ranges** - "Estimated $10K-$15K/month" not "$12.5K/month"
2. ✅ **Explain methodology** - Briefly describe how projections are calculated
3. ✅ **Show scenarios** - Present multiple growth scenarios side-by-side
4. ✅ **Provide exports** - Give stakeholders CSV files for their own analysis
5. ✅ **Set expectations** - Emphasize these are estimates, not commitments

---

## Known Limitations

1. **Data latency:** 45 min - 3 hour lag in `ACCOUNT_USAGE` views (Snowflake platform limitation)
2. **User tracking:** Some services lack individual user attribution (platform limitation)
3. **Historical data:** Limited to `ACCOUNT_USAGE` retention (typically 1 year)
4. **Projection assumptions:** Growth rates are user-defined estimates, not guarantees
5. **Token estimates:** Some token counts are approximations

**All limitations are platform constraints, not tool defects.**

---

## Support & Contributing

### Documentation

- `README.md` - Complete user guide (this file)
- `QUICKSTART.md` - 15-minute setup guide
- `docs/01-GETTING_STARTED.md` - Detailed getting started
- `docs/02-DEPLOYMENT_WALKTHROUGH.md` - Deployment walkthrough
- `docs/03-TROUBLESHOOTING.md` - Troubleshooting guide
- `diagrams/` - Architecture diagrams (data-flow, network-flow, auth-flow)

### Getting Help

1. Check `docs/03-TROUBLESHOOTING.md` for common issues
2. Review [Snowflake documentation](https://docs.snowflake.com)
3. For Streamlit issues, see [Streamlit docs](https://docs.streamlit.io)


---

**Disclaimer:**
This tool is provided as-is for cost estimation purposes. Projections are estimates based on user-defined assumptions and historical data. Actual costs may vary. Always verify credit pricing with your Snowflake account team and review actuals against projections regularly.

---

