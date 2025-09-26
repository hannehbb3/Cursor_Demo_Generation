
# 🚀 Cursor AI Starter Instructions for Snowflake Demo

PLEASE READ THE INSTRUCTIONS CAREFULLY

### 🏁 Step 0: Industry Selection & Demo Configuration

**FIRST: Snowflake Connection Setup**
Cursor AI should begin by asking the Solutions Engineer about their Snowflake connection:

```
🔗 Do you already have a Snowflake CLI connection configured?
- If YES: What is the name of your connection? (e.g., "my_cursor_snow", "demo_connection")
- If NO: We'll help you create one during the setup process.

Please provide your connection name: _______________
```

**THEN: Industry & Demo Selection**
After confirming the connection details, Cursor AI should prompt the Solutions Engineer to select a target industry, company name, and use case, if they have it--the use case is optional. If they also have them, ask for customer personas, and desired demo features. This selection will guide the customization of:
- Synthetic data generation
- Dashboard design
- WOW factor features
- Snowflake Intelligence integrations and it should include semantic model

**Industry Options:**
- Retail
- Healthcare
- Manufacturing
- Financial Services
- Media & Entertainment
- Logistics
- Other

Please select your industry: _______________
```

**Demo Features Prompt:**
Cursor AI should ask the user to specify which Snowflake features they want to showcase noting that it should be focusing on business outcomes:

```
🎯 Which Snowflake Intelligence features would you like to demonstrate?
□ Cortex ML (Anomaly Detection, Forecasting, Classification, Top Insights--e.g., https://docs.snowflake.com/en/guides-overview-ml-functions)
□ Cortex Analyst (https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-analyst)
□ Document AI (Text extraction and analysis)
□ Data Engineering (Snowpark, Tasks, Streams)
□ Semantic Layer (e.g., https://docs.snowflake.com/en/user-guide/views-semantic/overview, or https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-analyst/semantic-model-spec)
□ Advanced Analytics (e.g., Time Series, Geospatial, Data Metric Functions)
□ Data Sharing & Marketplace
□ Security & Governance features (Row Level Access Controls, Column Masking)

Please select your priorities: ___________
```

> 💡 *Cursor AI should use this selection to tailor the demo components using Snowflake best practices. Reference: https://docs.snowflake.com/
---

### 🎯 Objective

Build a **modular, reusable demo** using Cursor AI that:
- Integrates with Snowflake
- Customize it to the industry the solution engineer requested
- Showcases Snowflake Prime Features (easy, secure, governed--e.g., row base access control and column masking)
- Shows Snowflake Intelligence features (e.g., Cortex Analyst, Document AI)
- Includes synthetic data, dashboards, and interactive WOW elements

> 🧠 *Cursor AI should reference [Snowflake Documentation for all feature implementations.

### 🚨 CRITICAL WORKFLOW REQUIREMENT

⚠️ **For Streamlit Dashboard Success**: The synthetic data generation process MUST include loading data to Snowflake. All the data assets. The Streamlit application cannot function without data being present in Snowflake tables and views. The Streamlit Application should be loaded to Snowflake as well. 

**Required Workflow Order:**
1. Setup Snowflake connection
2. Create database, schema, and tables
3. **Generate synthetic data AND upload to Snowflake** (Step 3 - CRITICAL)
4. Create semantic views (depends on loaded data)
5. Implement Cortex ML features (depends on loaded data)
6. **Launch Streamlit dashboard** (depends on all above steps)

**❌ Common Failure Point**: Generating data but not loading it to Snowflake will result in empty dashboards and demo failure.

---

## 🧠 1. Professional Environment Setup with Snowflake CLI

⚠️ **IMPORTANT**: This setup uses Snowflake's official CLI with Conda virtual environments - the recommended approach for Solution Engineers to ensure isolated, conflict-free installations.

Cursor AI should create a folder structure to group files like:
-- Data (where all data files should be places if created)
-- Documentation (all documentation files like the "Demo Summary", "Data Model Architecture", "Clean up Guide", "Project Status", "ReadMe", "cursor_ai_snowflake_demo_instructions.md", and other pertinent documenation relevant to what we are building)
-- Python (to group all python scripts together)
-- SQL (to group all SQL scripts together)
-- Notebooks (if creating a notebook is best for the demo or hands on lab)

### 🔧 **Critical Setup Requirements**

**📋 Prerequisites:**
- **Cursor IDE**: Download and install first
- **Python 3**: Check with `python3 --version`
- **pip availability**: Check with `python3 -m pip --version`

**⚠️ Conda Virtual Environment (REQUIRED - Best Practice for SEs):**
```bash
# Create and activate Conda virtual environment for complete isolation
conda create -n snowflake_demo python=3.11 -y
conda activate snowflake_demo

# Verify activation
conda info --envs  # Should show active environment
which python  # Should show conda environment path
```

**🚀 Snowflake CLI Installation:**
```bash
# Install Snowflake CLI (snow) - the official tool
pip install snowflake-cli

# Verify installation
snow --version

# Install additional required packages in isolated environment
pip install snowflake-connector-python>=3.7.0
pip install snowflake-snowpark-python>=1.35.0
pip install streamlit==1.31.0
pip install pandas numpy plotly faker python-dateutil
```

**🔗 Snowflake CLI Connection Setup:**

**Step 1: Check for Existing Connections**
```bash
# First, check if you already have Snowflake connections configured
snow connection list
```

**❓ Do you already have a Snowflake connection configured?**

**If YES - Use Existing Connection:**
- What is the name of your existing connection? _______________
- Test your existing connection:
```bash
# Replace YOUR_CONNECTION_NAME with your actual connection name
snow connection test -c YOUR_CONNECTION_NAME
```
- If the test passes, you can use this connection for all demos
- If the test fails, proceed to create a new connection below

**If NO - Create New Connection:**
```bash
# Configure new Snowflake connection using CLI
snow connection add

# This will prompt for:
# - Connection name (e.g., "my_demo_connection", "my_cursor_snow")
# - Account identifier (format: ORG-ACCOUNT)
# - Username 
# - Authentication method (password/key-pair/SSO)  
# - Role (e.g., SVC_CURSOR_ROLE for demos)
# - Warehouse (COMPUTE_WH)
# - Database (CURSOR_DB) per compliance and security by Snowflake
# - Schema (leave blank for demos you can choose depending on the demo)

# Test the new connection
snow connection test --connection YOUR_NEW_CONNECTION_NAME
```

**✅ Connection Validation:**
Once you have a working connection, you'll see output similar to:
```
+-----------------------------------------------------------------------+
| key             | value                                               |
|-----------------+-----------------------------------------------------|
| Connection name | your_connection_name                                |
| Status          | OK                                                  |
| Host            | your-account.snowflakecomputing.com                 |
| Account         | your-account                                        |
| User            | your_username                                       |
| Role            | YOUR_ROLE                                           |
| Database        | not set (OK for demos)                             |
| Warehouse       | YOUR_WAREHOUSE                                      |
+-----------------------------------------------------------------------+
```

**📝 Remember Your Connection Name:**
Write down your connection name here: _______________
(You'll use this name for all demo scripts and configurations)

**⚠️ IMPORTANT FOR ALL SUBSEQUENT STEPS:**
Throughout this document, wherever you see `YOUR_CONNECTION_NAME`, replace it with your actual connection name. For example:
- If your connection is named `my_cursor_snow`, replace `YOUR_CONNECTION_NAME` with `my_cursor_snow`
- If your connection is named `demo_connection`, replace `YOUR_CONNECTION_NAME` with `demo_connection`
- This applies to all code examples, scripts, and command-line instructions

**📝 NOTE FOR CURSOR AI:**
When generating scripts and code examples, always use the connection name provided by the Solutions Engineer in Step 0. Replace all instances of `YOUR_CONNECTION_NAME` with their actual connection name throughout all generated files and scripts.

**🎯 Connection Best Practices for Solution Engineers:**
- **Use dedicated Conda environment** to avoid pip dependency conflicts
- **Never connect to customer or production data** - use demo/test accounts only
- **Enable Snowflake Cortex and Intelligence features** if available
- **Use descriptive connection names** for multiple demo scenarios

**📋 Connection Troubleshooting Guide:**
Common issues and solutions:
- ❌ **"Account not found"**: Use organization-account format (ORG-ACCOUNT)
- ❌ **"Authentication failed"**: Verify credentials in Snowflake web console first  
- ❌ **"Role not granted"**: Ensure your user has the specified role
- ❌ **"Connection timeout"**: Check network/VPN connection to Snowflake
- ❌ **"CLI command not found"**: Ensure Conda environment is activated

**🔄 Environment Management:**
```bash
# Activate environment for each session
conda activate snowflake_demo

# Deactivate when done
conda deactivate

# Remove environment when no longer needed
conda env remove -n snowflake_demo
```

> 📘 *Reference: [Snowflake CLI Documentation](https://docs.snowflake.com/en/developer-guide/snowflake-cli/installation/installation) and [Conda Best Practices](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)*

### 🚀 **Optional: Advanced MCP Integration with Cursor IDE**

⚡ **ENHANCED CAPABILITY**: For advanced users, integrate Snowflake's official Model Context Protocol (MCP) server to unlock powerful AI-assisted Snowflake operations directly in Cursor IDE.

**🎯 MCP Integration Benefits:**
- **Cortex AI Integration**: Direct access to Cortex Search, Analyst, and Agent services
- **Intelligent SQL Generation**: AI-powered SQL orchestration with safety controls
- **Object Management**: Smart Snowflake object operations via natural language
- **Semantic View Querying**: Discover and query semantic models seamlessly

**📦 MCP Server Installation:**
```bash
# In your activated conda environment
pip install snowflake-labs-mcp

# Verify installation
uvx snowflake-labs-mcp --help
```

**⚙️ MCP Configuration:**
Create a `mcp_config.yaml` file for your demo environment:
```yaml
# Cortex Services (configure based on your demo needs)
agent_services:
  - service_name: "demo_agent"
    description: "AI agent for demo data analysis and insights"
    database_name: "DEMO_INDUSTRY_DB"
    schema_name: "PUBLIC"

search_services:
  - service_name: "demo_search"
    description: "Search service for unstructured demo content"
    database_name: "DEMO_INDUSTRY_DB"
    schema_name: "PUBLIC"

analyst_services:
  - service_name: "demo_analyst"
    semantic_model: "DEMO_INDUSTRY_DB.PUBLIC.VW_BUSINESS_METRICS"
    description: "Business metrics analysis for demo scenarios"

# Tool Groups (enable as needed for demo)
other_services:
  object_manager: true    # Object creation/management
  query_manager: true     # SQL execution with safety controls
  semantic_manager: true  # Semantic view operations

# SQL Safety Controls (recommended for demos)
sql_statement_permissions:
  Select: true           # Allow data querying
  Create: true           # Allow object creation
  Insert: true           # Allow data insertion
  Update: false          # Disable updates for safety
  Delete: false          # Disable deletions for safety
  Drop: false           # Disable drops for safety
  All: false            # Don't allow all operations
```

**🔧 Cursor IDE MCP Setup:**
Add to your Cursor IDE configuration (`cursor-settings.json`):
```json
{
  "mcp": {
    "servers": {
      "snowflake-demo": {
        "command": "uvx",
        "args": [
          "snowflake-labs-mcp",
          "--service-config-file", "path/to/mcp_config.yaml",
          "--connection-name", "YOUR_CONNECTION_NAME"
        ]
      }
    }
  }
}
```

**✅ MCP Verification:**
```bash
# Test MCP server functionality
npx @modelcontextprotocol/inspector uvx snowflake-labs-mcp \
  --service-config-file "mcp_config.yaml" \
  --connection-name "YOUR_CONNECTION_NAME"
```

**🎨 Using MCP in Cursor:**
Once configured, you can use natural language commands in Cursor like:
- *"Create a customer analysis table using Cortex ML"*
- *"Query the sales semantic view for Q4 trends"*
- *"Generate synthetic customer data for the demo"*

> 📘 *Reference: [Snowflake MCP Server](https://github.com/Snowflake-Labs/mcp) and [MCP Introduction](https://modelcontextprotocol.io/introduction)*

---

### 🗂️ Step 2: Data Model & Semantic Layer

After completing the demo, Cursor AI should create a comprehensive **data architecture** for the demo environment. Additionally, it should provide a data model graphic representation in PDF format for documentation and customer presentation.

**📦 Database & Schema Creation**
```sql
CREATE DATABASE demo_industry_db;
CREATE SCHEMA based_on_industry;
CREATE STAGE demo_stage;
USE SCHEMA based_on_industry;
```

**🏗️ Table Creation Requirements:**
Create **5-7 interconnected tables** that pertain to the industry selected by solutions engineer:

**Industry Table Examples:**
- **Healthcare**: medications, patients, prescriptions, providers, adverse_events, clinical_trials
- **Retail**: customers, products, transactions, stores, inventory, suppliers  
- **Manufacturing**: machines, production_batches, maintenance_logs, quality_control, suppliers
- **Financial Services**: accounts, transactions, customers, loans, risk_assessments, compliance_reports

**⚠️ Critical Implementation Notes:**
- **Column Identifiers**: When using Snowpark to upload data, columns may require quoted identifiers in subsequent SQL queries
- **Data Relationships**: Ensure proper foreign key relationships between tables
- **Data Types**: Use appropriate Snowflake data types (VARIANT for JSON, proper DATE/TIMESTAMP formats)

**🧠 Semantic Layer Requirements:**
After creating base tables, Cursor AI must create business intelligence views:

```sql
-- Example semantic view structure
CREATE OR REPLACE VIEW VW_BUSINESS_METRICS AS
SELECT 
    entity."primary_key",
    entity."business_field",
    COUNT(DISTINCT related."key") as metric_count,
    AVG(related."value") as avg_metric
FROM base_table entity
LEFT JOIN related_table related ON entity."key" = related."foreign_key"
GROUP BY entity."primary_key", entity."business_field";
```

**📋 Required Business Views:**
1. **Safety/Quality Analytics** (industry-specific risk metrics)
2. **Performance Analytics** (operational efficiency metrics)  
3. **Financial Analytics** (revenue, cost, profitability metrics)
4. **Executive KPI Dashboard** (high-level business metrics)
5. **Predictive Analytics** (ML-ready data for Cortex)

> 📘 *Reference: [Snowflake Data Types](https://docs.snowflake.com/en/sql-reference/data-types) and [Semantic Layer Best Practices](https://docs.snowflake.com/en/user-guide/ui-snowsight-worksheets)* 

---

## 🧪 3. Synthetic Data Generation & Loading to Snowflake

⚠️ **CRITICAL REQUIREMENT**: Synthetic data MUST be loaded to Snowflake for the Streamlit application to work properly. The dashboard depends on data being available in Snowflake tables and views.

Use a Python script to generate realistic synthetic data for each table and **immediately load it to Snowflake**.

**📊 Data Generation Requirements:**
- Create **200+ rows** in each table for realistic analytics
- Use **industry-specific** data patterns and relationships
- Implement **proper referential integrity** between related tables
- Include **realistic business scenarios** (e.g., seasonal trends, anomalies)
- **MANDATORY**: Upload all generated data to Snowflake tables

**🔄 MANDATORY Data Upload Process:**

⚠️ **Without data loading, the Streamlit dashboard will show errors or empty visualizations!**

```python
# Complete data generation and upload workflow
from snowflake.snowpark import Session
from snowflake.connector import connect
import pandas as pd
import subprocess
import json

# Step 1: Generate synthetic data
def generate_synthetic_data():
    """Generate all synthetic DataFrames for the demo"""
    # Generate industry-specific data
    dataframes = {
        'table1': generate_table1_data(),
        'table2': generate_table2_data(),
        # ... etc for all tables
    }
    return dataframes

# Step 2: Create Snowflake session using CLI connection
def create_snowflake_session(connection_name="YOUR_CONNECTION_NAME"):
    """Create Snowflake session using CLI-configured connection"""
    try:
        # Get connection details from Snowflake CLI
        result = subprocess.run(
            ["snow", "connection", "test", "--connection", connection_name, "--format", "json"],
            capture_output=True, text=True, check=True
        )
        
        # Parse connection details (simplified - real implementation would extract actual config)
        print(f"✅ CLI connection '{connection_name}' verified")
        
        # Create session using Snowpark
        # Note: In practice, you'd extract actual connection parameters
        session = Session.builder.config("connection_name", connection_name).create()
        return session
        
    except subprocess.CalledProcessError as e:
        print(f"❌ CLI connection test failed: {e}")
        print("💡 Ensure you've run: snow connection add")
        raise Exception("Failed to create Snowflake session from CLI connection")

# Step 3: Upload ALL data to Snowflake (REQUIRED)
def upload_all_data_to_snowflake(dataframes, session):
    """Upload all DataFrames to Snowflake - REQUIRED for Streamlit"""
    print("🔄 Uploading synthetic data to Snowflake...")
    
    for table_name, df in dataframes.items():
        try:
            # Convert to Snowpark DataFrame
            snowpark_df = session.create_dataframe(df)
            
            # Upload with overwrite mode
            snowpark_df.write.mode('overwrite').save_as_table(table_name.upper())
            
            # Verify upload (CRITICAL for dashboard functionality)
            count_result = session.sql(f'SELECT COUNT(*) FROM {table_name.upper()}').collect()
            print(f"✅ {table_name}: {count_result[0][0]} rows uploaded to Snowflake")
            
        except Exception as e:
            print(f"❌ CRITICAL ERROR - Upload failed for {table_name}: {str(e)}")
            raise Exception(f"Data loading failed - Streamlit dashboard will not work!")

# Step 4: Execute complete workflow
def main():
    session = create_snowflake_session("YOUR_CONNECTION_NAME")
    dataframes = generate_synthetic_data()
    upload_all_data_to_snowflake(dataframes, session)  # MANDATORY
    print("🎉 All data loaded to Snowflake - Streamlit dashboard ready!")
```

**⚠️ Critical Data Generation Notes:**
- **Column Naming**: When Snowpark uploads data, column names may require quoted identifiers in subsequent SQL queries
- **Data Validation**: Test queries immediately after upload to verify data integrity
- **Dependency Order**: Upload parent tables before child tables to maintain referential integrity
- **JSON Fields**: Use proper JSON formatting for VARIANT columns

**🧪 MANDATORY Data Upload Verification:**

⚠️ **These verification steps are REQUIRED before launching the Streamlit dashboard!**

```python
# Complete verification workflow for dashboard readiness
def verify_data_for_streamlit(connection_name="YOUR_CONNECTION_NAME"):
    """Verify all data is properly loaded for Streamlit dashboard"""
    print("🔍 Verifying data upload for Streamlit dashboard...")
    
    # Create session for verification
    session = create_snowflake_session(connection_name)
    
    # Test 1: Verify all tables exist and have data
    required_tables = ['table1', 'table2', 'table3']  # Replace with actual table names
    for table in required_tables:
        try:
            count_result = session.sql(f'SELECT COUNT(*) FROM {table}').collect()
            row_count = count_result[0][0]
            if row_count > 0:
                print(f"✅ {table}: {row_count} rows loaded")
            else:
                raise Exception(f"❌ {table} is empty - Streamlit will show no data!")
        except Exception as e:
            print(f"❌ CRITICAL: {table} verification failed: {e}")
            raise Exception("Data loading incomplete - cannot proceed with Streamlit!")
    
    # Test 2: Verify data accessibility with quoted identifiers
    try:
        test_query = 'SELECT "column_name" FROM table_name LIMIT 5'
        result = session.sql(test_query).collect()
        print(f"✅ Data accessibility verified: {len(result)} rows returned")
    except Exception as e:
        print(f"❌ Data accessibility failed: {e}")
        print("Check column naming and quoted identifiers!")
    
    # Test 3: Verify semantic views work (if created)
    try:
        view_test = session.sql("SELECT COUNT(*) FROM VW_BUSINESS_METRICS LIMIT 1").collect()
        print(f"✅ Semantic views working - dashboard analytics ready")
    except Exception as e:
        print(f"⚠️ Semantic views not ready: {e}")
    
    # Test 4: Verify CLI connection health
    try:
        subprocess.run(
            ["snow", "connection", "test", "--connection", connection_name],
            check=True, capture_output=True
        )
        print(f"✅ CLI connection '{connection_name}' healthy")
    except subprocess.CalledProcessError as e:
        print(f"⚠️ CLI connection issue: {e}")
    
    print("🎉 Data verification complete - Streamlit dashboard ready to launch!")
    session.close()

# MANDATORY: Run verification before Streamlit
verify_data_for_streamlit("YOUR_CONNECTION_NAME")
```

**📋 Pre-Dashboard Launch Checklist:**
- [ ] All tables created in Snowflake
- [ ] All synthetic data uploaded (200+ rows per table)
- [ ] Data upload verification passed
- [ ] Table accessibility confirmed with queries
- [ ] Semantic views created and tested
- [ ] Snowflake session working properly

**🚨 Common Issues & Solutions:**
- **Empty Dashboard**: Data not uploaded to Snowflake → Run data upload script
- **Connection Errors**: CLI connection failed → Run `snow connection test --connection YOUR_CONNECTION_NAME`
- **Authentication Issues**: CLI credentials expired → Run `snow connection add` to reconfigure
- **No Charts**: Tables exist but empty → Verify data generation and upload
- **Query Errors**: Column naming issues → Use quoted identifiers in SQL queries
- **CLI Not Found**: Conda environment not activated → Run `conda activate snowflake_demo`

> 📘 *Reference: [Snowpark DataFrames](https://docs.snowflake.com/en/developer-guide/snowpark/python/working-with-dataframes) and [Data Loading Best Practices](https://docs.snowflake.com/en/user-guide/data-load-overview)*

---

### 🧠 Step 4: Snowflake Intelligence Features

Cursor AI should integrate Snowflake's advanced intelligence features to demonstrate real-world analytics and automation capabilities based on the user's selected features from Step 0.

**🧬 Cortex ML Functions**
Implement industry-specific ML use cases with proper error handling:

```sql
-- Anomaly Detection Example
SELECT 
    entity_id,
    metric_value,
    SNOWFLAKE.CORTEX.ANOMALY_DETECTION(metric_value) 
        OVER (PARTITION BY category ORDER BY date_column) as anomaly_score
FROM your_semantic_view
WHERE anomaly_score > 0.7;

-- Forecasting Example  
SELECT 
    date_column,
    actual_value,
    SNOWFLAKE.CORTEX.FORECAST(actual_value, date_column, 30) as forecasted_value
FROM time_series_view;

-- Classification Example
SELECT 
    record_id,
    SNOWFLAKE.CORTEX.CLASSIFY(text_content, ['high_risk', 'medium_risk', 'low_risk']) as risk_classification
FROM text_analysis_view;
```

**🗣️ Copilot Query Interface**
Enable natural language querying with industry-specific examples:

```python
# Copilot integration example
copilot_queries = [
    "Show me the top 5 highest risk entities this month",
    "What are the trends in our key performance indicators?", 
    "Find outliers in our operational data",
    "Compare performance metrics year over year"
]

# Test each query and provide business context
for query in copilot_queries:
    print(f"Natural Language Query: {query}")
    # Result would show in Snowflake Copilot interface
```

**📄 Document AI Implementation**
Simulate document parsing for industry-specific documents:

```sql
-- Document AI example for text extraction
SELECT 
    document_id,
    SNOWFLAKE.CORTEX.EXTRACT_ANSWER(document_text, 'What is the risk assessment?') as extracted_risk,
    SNOWFLAKE.CORTEX.SENTIMENT(document_text) as document_sentiment
FROM document_table;
```

**🧠 Industry-Specific Intelligence Use Cases**

Based on user's selected industry, implement use cases like the examples below or an specific use case per solutions engineer prompt:

- **Healthcare**: 
  - Drug discovery acceleration
  - Drug interaction detection using classification
  - Patient risk scoring with anomaly detection
  - Clinical outcome forecasting
  - Personalized medicine and treatment plans
  
- **Retail**: 
  - Demand forecasting for inventory optimization
  - Customer behavior anomaly detection  
  - Price optimization using ML
  - Hyper-personalization and predictive customer engagement
  - Generative AI for content creation

- **Financial Services**: 
  - Fraud detection with anomaly scoring
  - Credit risk assessment using classification
  - Market trend forecasting
  - AI-powered regulatory compliance
  - Personalized financial advice
  - Automated underwriting and credit decisions

- **Manufacturing**: 
  - Predictive maintenance using time series analysis
  - Quality control anomaly detection
  - Production optimization forecasting
  - Supply chain optimization

**✅ Verification Steps:**
```python
# Test Cortex ML functions with CLI connection
def test_cortex_features(connection_name="YOUR_CONNECTION_NAME"):
    session = create_snowflake_session(connection_name)
    try:
        # Test anomaly detection
        result = session.sql("SELECT SNOWFLAKE.CORTEX.ANOMALY_DETECTION(value) FROM test_view LIMIT 5").collect()
        print("✅ Cortex ML functions working")
        
        # Test CLI-based Cortex access
        subprocess.run(["snow", "cortex", "complete", "--connection", connection_name, 
                       "--model", "snowflake-arctic", "--prompt", "Test connection"], 
                      check=True, capture_output=True)
        print("✅ CLI Cortex integration working")
    except Exception as e:
        print(f"❌ Cortex ML error: {e}")
    finally:
        session.close()
```

> 📘 *Reference: [Snowflake Cortex ML](https://docs.snowflake.com/en/user-guide/snowflake-cortex/ml-functions), [Copilot](https://docs.snowflake.com/en/user-guide/ui-snowsight-copilot), [Document AI](https://docs.snowflake.com/en/user-guide/snowflake-cortex/llm-functions)*

---

### 📓 Step 5: Notebook vs External Components

Cursor AI should organize the demo into two main categories: **Notebook Components** and **External Components**. This separation ensures modularity and reusability.

---

#### 🧪 Notebook Components
These should be implemented directly in the Python notebook:
- **Synthetic Data Generation**  
  Scripts for creating and uploading synthetic data using Snowpark.
- **Snowflake Intelligence Features**  
  Examples of Cortex ML, Copilot queries, and Document AI.
- **Exploratory Analysis**  
  Basic visualizations, summary statistics, and insights per industry.
- **Markdown Explanations**  
  Inline documentation explaining each step and its relevance.

---

#### 🌐 External Components
These should be built outside the notebook and integrated as needed:
- **Streamlit Dashboard**  
  Interactive UI for visualizing data, toggling demo modes, and running simulations.
  ⚠️ **REQUIRES**: All synthetic data must be loaded to Snowflake BEFORE launching the dashboard!
- **SQL Setup Scripts**  
  Scripts for database, schema, and table creation.
- **Data Loading Scripts**  
  Scripts for generating and uploading synthetic data to Snowflake (REQUIRED for dashboard).
- **Deployment Instructions**  
  Steps to deploy the demo in a Snowflake-compatible environment (e.g., Snowpark Container Services, Streamlit sharing).

> 💡 *Cursor AI should generate modular code blocks and markdown cells for each component, and clearly label whether it belongs in the notebook or external UI.*

### 🎇 Step 6: WOW Factor

Cursor AI should include interactive and visually engaging features that elevate the demo experience and make it memorable for stakeholders. The demo should include a step by step presentation guideline for solutions engineer to pitch the Snowflake Platform to a potential customer. 

---

#### 🧪 What-If Simulator
Allow users to simulate changes in key variables (e.g., supplier reliability, inventory levels) and observe downstream effects.

**Example:**
- Slider to adjust `reliability_score`
- Recalculate impact on `shipments`, `orders`, or `risk_scores`

> 💡 *Use Streamlit widgets and Snowflake queries to power simulations.*

---

#### 💬 Copilot Chat Interface
Embed a natural language interface using Snowflake Copilot to allow users to ask questions like:
> “Which suppliers are most reliable in the Northeast region?”
> “Show me inventory levels below reorder threshold.”

> 📘 *Reference: [Snowflake Copilot](https://docs.snowflake.com/en/user-guide/copilot.htmlreamlit dashboard to switch between:
- **Live Mode**: Real-time interaction
- **Demo Mode**: Preloaded responses and visuals for fast walkthroughs

---

#### 🧠 Industry-Specific WOW Ideas
If Solutions Engineer did not provide a use case for the demo, then Cursor AI should adapt WOW features based on the selected industry:
- **Healthcare**: Simulate patient risk scoring
- **Retail**: Forecast sales based on promotions
- **Manufacturing**: Predict machine failure from sensor data
- **Financial Services**: Simulate credit risk changes

> 💡 *Cursor AI should explain each WOW feature in markdown and link it to Snowflake capabilities.*


### 🎉 Step 7: Fun Elements

Cursor AI should include lighthearted and engaging features that add personality to the demo and make it more enjoyable to explore.

---

### 🧹 Step 8: MANDATORY Demo Cleanup (CRITICAL)

⚠️ **This step is REQUIRED and CANNOT be skipped** ⚠️

Cursor AI MUST create comprehensive cleanup procedures to remove ALL demo objects from Snowflake. This is mandatory to prevent ongoing charges and maintain account hygiene.

#### **Required Cleanup Components:**

**1. Automated Python Cleanup Script (`cleanup_demo.py`)**
```python
# Interactive cleanup with confirmation prompts
# Step-by-step progress tracking
# Comprehensive error handling and verification
# Fallback options if automation fails
```

**2. SQL-Only Cleanup Script (`cleanup_demo.sql`)**
```sql
-- Direct SQL commands for manual execution
-- Dependency-aware object removal order
-- Can be run in any Snowflake client or worksheet
```

**3. Quick Shell Script (`cleanup.sh`)**
```bash
# One-command execution for solutions engineers
# Virtual environment handling
# Exit status checking
```

**4. Emergency One-Liner**
```sql
-- For immediate cleanup if other methods fail
DROP DATABASE IF EXISTS [DEMO_DATABASE_NAME];
```

#### **Cleanup Documentation Requirements:**

- **Dedicated CLEANUP_GUIDE.md** with step-by-step instructions
- **Prominent cleanup section** in main README
- **Post-demo cleanup reminders** in demo walkthrough
- **Verification steps** to confirm complete removal

#### **Objects That MUST Be Removed:**
- All synthetic data tables and records
- All business intelligence views and semantic layers
- Complete demo database and schemas
- Any temporary or staging objects
- All Cortex ML models and functions

#### **Timeline and Best Practices:**
- Cleanup should be executed **within 24 hours** of demo completion
- Include **interactive confirmation** before destructive operations
- Provide **progress tracking** and error handling
- Include **verification queries** to confirm successful cleanup
- Emphasize **cost impact** - cleanup prevents ongoing Snowflake charges

---

### 🧾 Step 9: Success Verification & Professional Documentation

Cursor AI should ensure the final demo meets enterprise-grade standards and includes comprehensive verification.

---

#### ✅ **Mandatory Success Verification**

Cursor AI must verify each component works before completing the demo:

```python
# Complete verification script
def verify_demo_success(connection_name="YOUR_CONNECTION_NAME"):
    """Comprehensive demo verification using CLI connection"""
    verification_results = {}
    session = None
    
    try:
        # 1. Test CLI connection
        try:
            subprocess.run(["snow", "connection", "test", "--connection", connection_name], 
                         check=True, capture_output=True)
            verification_results['cli_connection'] = f"✅ CLI connection '{connection_name}' working"
        except subprocess.CalledProcessError as e:
            verification_results['cli_connection'] = f"❌ CLI connection failed: {e}"
        
        # 2. Create session and test Snowflake connection
        session = create_snowflake_session(connection_name)
        try:
            version = session.sql("SELECT CURRENT_VERSION()").collect()[0][0]
            verification_results['snowflake_connection'] = f"✅ Connected to Snowflake {version}"
        except Exception as e:
            verification_results['snowflake_connection'] = f"❌ Snowflake connection failed: {e}"
        
        # 3. Verify all tables have data
        tables = ['table1', 'table2', 'table3']  # Replace with actual table names
        for table in tables:
            try:
                count = session.sql(f'SELECT COUNT(*) FROM {table}').collect()[0][0]
                verification_results[f'table_{table}'] = f"✅ {table}: {count} records"
            except Exception as e:
                verification_results[f'table_{table}'] = f"❌ {table}: {e}"
        
        # 4. Test semantic views
        views = ['VW_SAFETY_ANALYTICS', 'VW_PERFORMANCE_METRICS']  # Replace with actual views
        for view in views:
            try:
                count = session.sql(f'SELECT COUNT(*) FROM {view}').collect()[0][0]
                verification_results[f'view_{view}'] = f"✅ {view}: {count} rows"
            except Exception as e:
                verification_results[f'view_{view}'] = f"❌ {view}: {e}"
        
        # 5. Test Cortex ML functions (if selected)
        try:
            result = session.sql("SELECT SNOWFLAKE.CORTEX.ANOMALY_DETECTION(value) FROM test_view LIMIT 1").collect()
            verification_results['cortex_ml'] = "✅ Cortex ML functions working"
        except Exception as e:
            verification_results['cortex_ml'] = f"⚠️ Cortex ML: {e}"
            
        # 6. Test CLI Cortex access (if available)
        try:
            subprocess.run(["snow", "cortex", "complete", "--connection", connection_name,
                           "--model", "snowflake-arctic", "--prompt", "Test"], 
                          check=True, capture_output=True, timeout=30)
            verification_results['cli_cortex'] = "✅ CLI Cortex access working"
        except (subprocess.CalledProcessError, subprocess.TimeoutExpired) as e:
            verification_results['cli_cortex'] = f"⚠️ CLI Cortex: {e}"
    
    finally:
        if session:
            session.close()
    
    return verification_results

# Execute verification
results = verify_demo_success("YOUR_CONNECTION_NAME")
for component, status in results.items():
    print(status)
```

---

#### 📋 **Professional Documentation Requirements**

Cursor AI must create enterprise-ready documentation:

**1. Semantic Model Documentation (`semantic_model.yaml`)**
```yaml
# Include comprehensive business definitions
metadata:
  version: "1.0"
  domain: "Selected Industry"
  description: "Business semantic layer"

entities:
  # Define each business entity with clear descriptions
  
metrics:
  # Define all KPIs and business calculations
  
business_rules:
  # Document business logic and validation rules
```

**2. Data Architecture Documentation (`data_model_documentation.md`)**
- Entity relationship diagrams
- Table specifications with business rules
- Data lineage and governance framework
- Integration points and APIs

**3. Executive Summary (`demo_summary.md`)**
- Business value proposition
- Technical architecture overview
- Demo walkthrough scenarios
- Next steps and ROI analysis

**4. Setup and Deployment Guide**
- Complete environment setup instructions
- Troubleshooting guide with common issues
- Verification steps and success criteria
- Customer deployment checklist

**5. MANDATORY Demo Cleanup (CRITICAL)**
- ⚠️ **ALWAYS create comprehensive cleanup scripts** - this is REQUIRED by Snowflake demo guidelines
- **Multiple cleanup options** must be provided for solutions engineers:
  - Automated Python cleanup script with confirmation prompts
  - SQL-only cleanup script for manual execution
  - Quick shell script for one-command cleanup
  - Emergency one-liner SQL command
- **Cleanup must remove ALL demo objects** to prevent ongoing charges:
  - All synthetic data tables and records
  - All business intelligence views and semantic layers
  - Complete demo database and schemas
  - Any temporary or staging objects
- **Documentation requirements** for cleanup:
  - Dedicated cleanup guide (CLEANUP_GUIDE.md)
  - Clear instructions in main README
  - Prominent cleanup reminders in demo walkthrough
  - Verification steps to confirm complete removal
- **Best practices** for cleanup implementation:
  - Interactive confirmation before destructive operations
  - Progress tracking and error handling
  - Verification queries to confirm successful cleanup
  - Fallback manual commands if automation fails
- **Timeline:** Cleanup should be executed within 24 hours of demo completion
- **Cost impact:** Emphasize that cleanup prevents ongoing Snowflake storage and compute charges

---

#### 🧩 **Modularity & Reusability**
- Each component (data generation, intelligence features, dashboards) should be independently reusable
- Use clear function definitions and markdown explanations to separate logic
- Include configuration files for easy customization across industries

---

#### ✅ **Enterprise Compatibility**
- All components must be Snowflake-compatible:
  - Use Snowpark for Python integration
  - Use proper Snowflake SQL syntax with quoted identifiers
  - Follow Snowflake security and governance best practices
  - Include proper error handling and logging

**📋 Final Deployment Checklist:**
- [ ] All tables created and populated (200+ rows each)
- [ ] **CRITICAL: All synthetic data uploaded to Snowflake (REQUIRED for Streamlit)**
- [ ] Data upload verification completed successfully
- [ ] All semantic views working with business-friendly metrics
- [ ] Snowflake Intelligence features tested and functional
- [ ] **CRITICAL: Data availability confirmed before launching Streamlit dashboard**
- [ ] Interactive dashboard deployed and accessible with populated data
- [ ] Professional documentation complete
- [ ] End-to-end verification successful (including data loading)
- [ ] Demo scenarios tested and validated with real data
- [ ] **MANDATORY: Comprehensive cleanup scripts created and tested**
- [ ] **MANDATORY: Cleanup documentation provided (CLEANUP_GUIDE.md)**
- [ ] **MANDATORY: Cleanup instructions prominently featured in README**
- [ ] **MANDATORY: Post-demo cleanup reminders in walkthrough guide**

---

## 🚨 **CRITICAL LESSONS LEARNED - AVOID COMMON PITFALLS**

*This section contains essential guidance to prevent the most common issues encountered during Snowflake demo development.*

### **⚠️ Issue #1: Snowflake Column Identifier Consistency**

**PROBLEM**: Mixed quoted/unquoted identifiers and case sensitivity causing SQL compilation errors.

**SOLUTION - MANDATORY APPROACH**:
```sql
-- ✅ CORRECT: Use consistent quoted lowercase identifiers
CREATE TABLE ROUTES (
    route_id VARCHAR(20),           -- Unquoted = stored as UPPERCASE
    planned_miles INTEGER,          -- Unquoted = stored as UPPERCASE  
    route_efficiency_score FLOAT   -- Unquoted = stored as UPPERCASE
);

-- ✅ CORRECT: Query with quoted lowercase (Snowflake auto-matches)
SELECT 
    r."route_id",
    r."planned_miles", 
    r."route_efficiency_score"
FROM ROUTES r;

-- ❌ WRONG: Mixing quoted and unquoted
SELECT r.route_id, r."PLANNED_MILES" FROM ROUTES r;  -- WILL FAIL
```

**ENFORCE THIS PATTERN**:
- CREATE tables with **unquoted lowercase** column names
- QUERY tables with **quoted lowercase** column names  
- NEVER mix quoted/unquoted in same query
- TEST every SQL query in Snowflake before adding to dashboard

### **⚠️ Issue #2: Streamlit DataFrame Column References**

**PROBLEM**: Python code expecting different case than Snowflake returns.

**SOLUTION**:
```python
# ✅ CORRECT: Snowflake returns uppercase for unquoted columns
route_data = load_data_from_snowflake(query, session)
# DataFrame columns will be: ['ROUTE_ID', 'PLANNED_MILES', 'ROUTE_EFFICIENCY_SCORE']

# ✅ Use uppercase in Python code
route_data['ROUTE_EFFICIENCY_SCORE']
route_data['PLANNED_MILES'] 

# ❌ WRONG: Using lowercase
route_data['route_efficiency_score']  # KeyError!
```

**TESTING APPROACH**:
```python
# ALWAYS test column names immediately after loading data
st.write(f"DEBUG: Columns returned: {list(df.columns)}")
st.write(f"DEBUG: Data shape: {df.shape}")
st.write(f"DEBUG: Sample data: {df.head(2)}")
```

### **⚠️ Issue #3: Plotly Chart Compatibility** 

**PROBLEM**: Some Plotly charts may not render in certain Streamlit environments.

**SOLUTION - DUAL APPROACH**:
```python
# ✅ RECOMMENDED: Use matplotlib for reliable rendering
import matplotlib.pyplot as plt

def create_reliable_chart(data, chart_type='histogram'):
    if chart_type == 'histogram':
        fig, ax = plt.subplots(figsize=(10, 6))
        ax.hist(data['column'], bins=20, alpha=0.7, edgecolor='black')
        ax.set_title('Professional Title')
        ax.grid(True, alpha=0.3)
        st.pyplot(fig)
    
    elif chart_type == 'scatter':
        fig, ax = plt.subplots(figsize=(10, 6))
        scatter = ax.scatter(data['x'], data['y'], c=data['color'], cmap='viridis')
        plt.colorbar(scatter)
        st.pyplot(fig)

# ⚠️ ALTERNATIVE: Plotly with fallback
try:
    fig = px.histogram(data, x='column')
    st.plotly_chart(fig)
except Exception as e:
    st.warning(f"Plotly issue: {e}. Using matplotlib fallback.")
    create_reliable_chart(data, 'histogram')
```

### **⚠️ Issue #4: Import Organization**

**SOLUTION - COMPLETE IMPORT BLOCK**:
```python
# ✅ MANDATORY: Include ALL imports at top
import streamlit as st
import pandas as pd
import plotly.express as px
import plotly.graph_objects as go
import matplotlib.pyplot as plt  # CRITICAL for chart fallbacks
import numpy as np
from snowflake.snowpark import Session
import subprocess
import json
import random

# CLI-based connection function for Streamlit
def get_snowflake_session(connection_name="YOUR_CONNECTION_NAME"):
    """Get Snowflake session using CLI connection for Streamlit"""
    try:
        # Verify CLI connection is working
        subprocess.run(["snow", "connection", "test", "--connection", connection_name], 
                      check=True, capture_output=True)
        
        # Create session using CLI connection
        session = Session.builder.config("connection_name", connection_name).create()
        return session
    except Exception as e:
        st.error(f"❌ Failed to connect to Snowflake via CLI: {e}")
        st.info("💡 Ensure your CLI connection is configured: `snow connection add`")
        return None
```

### **⚠️ Issue #5: Data Validation & Testing**

**MANDATORY TESTING PATTERN**:
```python
@st.cache_data
def load_data_with_validation(query, connection_name="YOUR_CONNECTION_NAME"):
    """Load data with comprehensive validation using CLI connection"""
    try:
        # Get session using CLI connection
        session = get_snowflake_session(connection_name)
        if not session:
            return pd.DataFrame()
            
        result = session.sql(query).collect()
        df = pd.DataFrame([row.as_dict() for row in result])
        
        # CRITICAL: Validate data immediately
        if df.empty:
            st.error(f"❌ Query returned no data: {query[:100]}...")
            return pd.DataFrame()
            
        # Log for debugging (remove in production)
        st.write(f"✅ Data loaded: {df.shape} rows, columns: {list(df.columns)}")
        
        session.close()
        return df
    except Exception as e:
        st.error(f"❌ SQL Error: {str(e)}")
        st.code(query)  # Show problematic query
        return pd.DataFrame()
```

### **⚠️ Issue #6: Environment & Dependencies**

**SOLUTION - COMPLETE SETUP**:
```bash
# ✅ MANDATORY: Test setup sequence using Conda
conda create -n snowflake_demo python=3.11 -y
conda activate snowflake_demo
pip install snowflake-cli streamlit pandas plotly matplotlib snowflake-snowpark-python
pip freeze > requirements.txt

# ✅ ALWAYS configure and test CLI connection first
snow connection add
snow connection test --connection YOUR_CONNECTION_NAME

# ✅ ALWAYS test data upload before dashboard
python upload_data.py

# ✅ ONLY THEN launch dashboard
streamlit run dashboard.py
```

### **⚠️ Issue #7: Debugging Strategy**

**DEVELOPMENT APPROACH**:
```python
# ✅ PHASE 1: Build with debugging
def create_chart_with_debug(data, chart_config):
    st.write(f"🔍 DEBUG: Data shape: {data.shape}")
    st.write(f"🔍 DEBUG: Columns: {list(data.columns)}")
    st.write(f"🔍 DEBUG: Data types: {data.dtypes}")
    
    # Build chart
    try:
        # Chart creation code
        pass
    except Exception as e:
        st.error(f"Chart error: {e}")
        st.write("Data sample:")
        st.write(data.head())

# ✅ PHASE 2: Remove debugging for production
def create_chart_production(data, chart_config):
    # Clean, production-ready chart code only
    pass
```

---

**🎯 PREVENTION CHECKLIST**:
- [ ] ✅ All table schemas use unquoted lowercase column names
- [ ] ✅ All SQL queries use quoted lowercase column references  
- [ ] ✅ All Python DataFrame access uses uppercase column names
- [ ] ✅ matplotlib imports included for chart fallbacks
- [ ] ✅ Every SQL query tested in Snowflake before dashboard integration
- [ ] ✅ Data validation logic included in all data loading functions
- [ ] ✅ Debug output included during development, removed for production
- [ ] ✅ Complete import block at top of all Python files
- [ ] ✅ Environment tested with minimal example before building full demo
- [ ] ✅ Cleanup scripts tested to ensure complete removal

> 🚨 **CRITICAL**: Following these patterns will prevent 95% of common issues and save hours of debugging time.

---

> 📘 *Reference: [Snowflake Developer Guide](https://docs.snowflake.com/en/developer-guide), [Semantic Layer Best Practices](https://docs.snowflake.com/en/user-guide/ui-snowsight-worksheets), [Enterprise Deployment Guide](https://docs.snowflake.com/en/user-guide/admin-account-identifier)*

> 💡 **Final Requirements**: 
> 1. Cursor AI should provide a README or markdown summary that enables a Solutions Engineer to confidently demonstrate the solution to enterprise customers within 30 minutes, including technical deep-dives and business value discussions.
> 2. **MANDATORY**: Comprehensive cleanup scripts and documentation must be provided to ensure complete removal of all demo objects from Snowflake after demo completion. This is not optional - it's required by Snowflake demo guidelines to prevent ongoing charges.