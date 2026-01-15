# Getting Started: Cortex Cost Calculator

**Professional cost tracking and forecasting for Snowflake Cortex workloads**

**⏱️ Time to Deploy:** 15 minutes  
**💰 Value:** Accurate cost projections, budget planning, multi-scenario analysis

---

## 👋 What You'll Build

Deploy this toolkit and you'll have:

- ✅ **Real-time tracking** - 16 views monitoring all Cortex services (Analyst, Search, Functions, Document AI, Fine-tuning)
- ✅ **Historical snapshots** - Automated daily captures with trend analysis
- ✅ **Cost projections** - Multi-scenario forecasting (3, 6, 12, 24 months)
- ✅ **Interactive calculator** - Streamlit app deployed from Git
- ✅ **Query-level analysis** - Identify expensive individual queries
- ✅ **Export-ready estimates** - For proposals and finance teams

---

## 🎯 Two Ways to Use This Tool

### For Solution Engineers (Two-Account Workflow)

```
Customer Account → Deploy Monitoring → Wait 7-14 days → Extract CSV
        ↓
Your Account → Deploy Calculator → Upload CSV → Generate Estimates → Sales Team
```

**Time Investment:**
- One-time setup in your account: 10 minutes
- Per-customer analysis: 5-10 minutes

**Benefits:**
- One calculator handles unlimited customers
- Professional, repeatable cost estimates
- No data leaves customer's Snowflake account
- Export-ready for proposals

### For Customers (Self-Service)

```
Your Account → Deploy Monitoring + Calculator → Real-time Analysis → Budget Planning
```

**Time Investment:**
- Initial setup: 15 minutes
- Ongoing analysis: Instant

**Benefits:**
- Ongoing cost visibility
- Track actual vs projected
- Department cost allocation
- Finance team self-service

---

## 📋 Prerequisites

Before starting, ensure you have:

- ✅ **Snowflake account** with Cortex usage (ideally 7-14 days of history)
- ✅ **ACCOUNTADMIN role** OR role with `IMPORTED PRIVILEGES` on `SNOWFLAKE` database
- ✅ **Active warehouse** for running queries
- ✅ **Access to Snowsight** (Snowflake's web interface)

### Quick Access Test

Run this query to verify you have the required permissions:

```sql
SELECT COUNT(*) 
FROM SNOWFLAKE.ACCOUNT_USAGE.METERING_DAILY_HISTORY 
WHERE usage_date >= DATEADD('day', -7, CURRENT_DATE());
```

**Expected:** A number (even if 0) - not an error message

**If error:** Grant privileges first:
```sql
USE ROLE ACCOUNTADMIN;
GRANT IMPORTED PRIVILEGES ON DATABASE SNOWFLAKE TO ROLE <YOUR_ROLE>;
```

---

## 🚀 Quick Deployment (Recommended)

**Option A: Deploy Everything in One Step (~2 minutes)**

Copy/paste [`deploy_all.sql`](../deploy_all.sql) into Snowsight → Click "Run All"

This deploys:
- API Integration for GitHub access
- Git Repository with project code
- 16 monitoring views
- Snapshot table + serverless task
- Streamlit calculator app

**Skip to Step 3 below after deployment completes.**

---

## 🛠️ Step-by-Step Deployment (Alternative)

**Option B: Deploy Monitoring First, Then Calculator (~3-5 minutes)**

### Step 1: Deploy Monitoring Views (~1 minute)

This creates 16 read-only views that track all Cortex service usage.

### 1.1 Access Snowflake

1. Navigate to [https://app.snowflake.com](https://app.snowflake.com)
2. Log in with your credentials
3. Click **Worksheets** in the left navigation
4. Create a new worksheet

### 1.2 Run Deployment Script

1. Open `sql/01_deployment/deploy_cortex_monitoring.sql` from this project
2. Copy the entire file contents
3. Paste into your Snowflake worksheet
4. Click **"Run All"** (or press `Ctrl+Enter` to run all statements)

### 1.3 Verify Deployment

Watch for these success messages:

- ✅ Database `SNOWFLAKE_EXAMPLE` created
- ✅ Schema `CORTEX_USAGE` created
- ✅ 9 views created successfully
- ✅ Validation queries showing row counts

**What got created:**
```
SNOWFLAKE_EXAMPLE.CORTEX_USAGE
├── V_CORTEX_ANALYST_DETAIL
├── V_CORTEX_SEARCH_DETAIL
├── V_CORTEX_SEARCH_SERVING_DETAIL
├── V_CORTEX_FUNCTIONS_DETAIL
├── V_CORTEX_FUNCTIONS_QUERY_DETAIL
├── V_DOCUMENT_DETAIL
├── V_CORTEX_DAILY_SUMMARY           ← Main rollup view
├── V_CORTEX_COST_EXPORT             ← Pre-formatted for calculator
└── V_METERING_SERVICES              ← High-level validation
```

**⚠️ Note:** Views may show 0 rows if:
- No Cortex usage in last 90 days
- Data latency (wait 3 hours after usage)
- This is normal and won't prevent deployment

---

## 📊 Step 2: Deploy Calculator (5 minutes)

Deploy the interactive Streamlit calculator in Snowflake.

### 2.1 Create Streamlit App

In Snowsight:

1. Click **"Projects"** in left navigation
2. Click **"Streamlit"**
3. Click **"Apps"**
4. Click **"+ Streamlit App"** button (top right)

### 2.2 Configure App

Fill in the form:

| Field | Value | Notes |
|-------|-------|-------|
| **App name** | `CORTEX_COST_CALCULATOR` | Or your preferred name |
| **App location** | `SNOWFLAKE_EXAMPLE.CORTEX_USAGE` | Must match where views were deployed |
| **Warehouse** | Select your warehouse | SMALL is sufficient |

### 2.3 Add Application Code

1. Open `streamlit/cortex_cost_calculator/streamlit_app.py` from this project
2. Copy the **entire file contents**
3. In Snowflake, paste into the code editor (replacing the default code)

### 2.4 Add Package Dependencies

1. Click the **"Packages"** tab in the Snowflake editor
2. Open `streamlit/cortex_cost_calculator/environment.yml` from this project
3. Copy the dependencies section
4. Add the packages to the Snowflake packages field

### 2.5 Launch the App

1. Click **"Create"** button (bottom right)
2. Wait 30 seconds for the app to initialize
3. The app will automatically launch when ready

**🎉 Success!** You now have a fully functional cost calculator.

---

## 💰 Step 3: Analyze Your Costs (5 minutes)

Now let's use the calculator to understand your Cortex spending.

### 3.1 Configure Data Source

In the calculator sidebar:

**For same-account deployment (customers):**
- **Data Source:** Select **"Query Views (Same Account)"**
- **Lookback Period:** 30 days (adjust as needed)
- **Credit Cost:** Update to your actual credit price (default: $3.00)

**For SE workflow (analyzing customer data):**
- **Data Source:** Select **"Upload Customer CSV"**
- Upload the CSV file extracted from customer account
- Credit cost will be configured per analysis

### 3.2 Review Historical Analysis

Click the **"📈 Historical Analysis"** tab:

**You'll see:**
- **Summary metrics:** Total credits, total cost, avg daily usage
- **Service breakdown:** Which services consume the most credits
- **Usage trends:** Daily credit consumption over time
- **Service distribution:** Pie chart showing cost allocation

**Use this to:**
- Validate data quality
- Understand current spending patterns
- Identify which services drive costs
- Detect usage anomalies

### 3.3 Generate Cost Projections

Click the **"🔮 Cost Projections"** tab:

**Configure projections:**
1. **Projection Period:** Choose 3, 6, 12, or 24 months
2. **Monthly Growth Rate:** Use slider (0-100%)
3. **Pre-defined scenarios:**
   - **Conservative (10%):** Steady adoption, existing use cases
   - **Moderate (25%):** Active expansion, new features
   - **Aggressive (50%):** Rapid rollout, multiple teams
   - **Rapid (100%):** Explosive growth, company-wide adoption

**You'll see:**
- **Month 1 vs Month 12 costs**
- **Total year cost** with variance range
- **Interactive chart** with confidence bands
- **Monthly breakdown table**

### 3.4 Model User Personas (Multi-User Calculator)

Scroll down to the **"💰 Cost per User Calculator"** section:

**Define your user types:**
1. **Add/Edit Personas:**
   - Default: Power User (5 @ 2.0x), Regular User (15 @ 1.0x), Casual User (10 @ 0.3x)
   - Click ➕ **Add Persona** to add more
   - Click 🗑️ to remove personas
   - Adjust user counts and intensity multipliers

2. **Set Baseline Usage:**
   - Operations per user per month for each service
   - Pre-filled with historical averages
   - All personas multiply from this baseline

**You'll see:**
- **Total users** across all personas
- **Total monthly cost** breakdown
- **Per-persona costs** with detailed tables
- **Budget capacity** with scaling options

**Use this for:**
- Modeling 10-30 test users with different usage patterns
- Department cost allocation
- Optimizing user mix for budget constraints

### 3.5 Compare Scenarios

Click the **"📊 Scenario Comparison"** tab:

- **Side-by-side** comparison of all growth scenarios
- **Custom scenario builder** with specific parameters
- **Export comparison table** for stakeholder presentations

### 3.6 Export Estimates

Click the **"📋 Summary Report"** tab:

1. Review **credit breakdown by service**
2. Click **"Download Credit Estimate (CSV)"**
3. Open in Excel for proposals or budget documents

**The CSV includes:**
- Service-by-service breakdown
- Daily and monthly projections
- Cost estimates at current credit price
- Notes on methodology

