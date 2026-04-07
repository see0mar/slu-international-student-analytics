
🎓 SLU International Student Enrollment Analytics Pipeline
End-to-End Analytics Engineering: From Fragmented Data to Strategic Business Insights


Final Looker Studio Dashboard showing the International Student Funnel.
(https://lookerstudio.google.com/s/neO3QSNMtJc)

📊 Executive Summary
This project involved designing and implementing a production-grade data pipeline for Saint Louis University. By centralizing 46,652 records from four disconnected legacy systems into a structured PostgreSQL Star Schema, I unblocked critical recruitment decisions and identified significant revenue recovery opportunities.
🎯 Business Impact
💰 $561K Value Recovery: Identified 39 "lost" visa-approved students previously missing from enrollment tracking.
⏱️ 240 Hours/Year Saved: Automated manual Excel reporting workflows into a real-time BI solution.
📊 Strategic De-risking: Resolved a 99.7% data gap in geographic reporting, unblocking a $250K recruitment budget.
🎓 Funnel Transparency: Created the first unified view of 6,893 students across the entire lifecycle (App → Deposit → Visa).


🏗️ Technical Architecture
graph LR

    A[Raw CSVs] --> B[Python/Pandas ETL]

    B --> C[PostgreSQL Star Schema]

    C --> D[Materialized Views]

    D --> E[Looker Studio Dashboard]


📁 Repository Structure
slu-international-student-analytics/

├── README.md                        # Project documentation

├── notebooks/

│   ├── 01_etl_data_cleaning.ipynb   # Extraction and automated cleaning

│   ├── 02_data_profiling_eda.ipynb  # Exploratory analysis & quality checks

│   └── requirements.txt             # Python environment dependencies

├── sql/

│   ├── schema_ddl.sql               # Database architecture (DDL)

│   ├── materialized_views.sql       # Performance optimization logic

│   └── validation_suite.sql         # Automated data integrity checks

├── images/                          # Dashboard screenshots and ERDs

└── docs/

    ├── DATA_DICTIONARY.md           # Documentation for 179 mapped columns

    └── QUALITY_REPORT.md            # Log of data anomalies and fixes


🛠️ Phase 1: Data Engineering & Quality Assurance
Objective: Transform "dark data" into a reliable, queryable asset.

The Duplicate Problem: Discovered the Connect dataset was an event log (79.98% "duplicates"). Developed a Python deduplication logic to extract the latest status snapshot for each student.
The Identity Linkage Challenge: Achieved an 85.8% match rate (up from 23.6%) by implementing a multi-key joining strategy between Applicant IDs and SEVIS IDs.

# snippet of the deduplication logic used to handle event-log data

connect_latest = (connect_raw

                  .sort_values('event_timestamp')

                  .groupby('reference_id')

                  .tail(1))


🗄️ Phase 2: Database Modeling (Star Schema)
Instead of a "flat file" approach, I architected a relational model to ensure referential integrity and query performance.

Key Features:

Normalization: 3 Dimension tables (dim_applicant, dim_sevis, dim_outreach) and 1 Fact table.
Performance Engineering: Implemented Materialized Views with 7 strategic indexes, reducing dashboard latency from 5 seconds to <1 second.

-- Strategic indexing for high-traffic BI filters

CREATE INDEX idx_mv_visa_status ON fact_student_status_mv(has_visa);

CREATE INDEX idx_mv_region ON fact_student_status_mv(citizenship);


📈 Phase 3: BI & Strategic Analytics
The final dashboard provides four layers of visibility:

Conversion Funnel: Real-time tracking of the 78% drop-off at the deposit stage.
Geographic Intelligence: Heatmaps identifying India, Nigeria, and Ghana as the top 71% of the applicant pool.
ROI Analysis: Comparison of INTO Partnership vs. Slate Direct applications, showing INTO has 2x the conversion rate.
Financial Forecasting: Standardized scholarship distribution analysis.


🔍 Data Quality Framework
Issue Identified
Technical Solution
Business Outcome
99.7% Missing Country Data
Backfilled via SEVIS linkage & fuzzy matching.
Unblocked geographic marketing spend.
Orphaned Visa Records
Cross-referenced Outreach status with Applicant IDs.
$500K+ in potential revenue recovered.
Latency Issues
Transitioned from live joins to Materialized Views.
Improved executive adoption of BI tools.



🚀 Getting Started
1. Prerequisites
Python 3.8+
PostgreSQL 13+
Looker Studio account
2. Installation & Setup
# Clone the repository

git clone https://github.com/see0mar/slu-international-student-analytics.git

# Install dependencies

pip install -r notebooks/requirements.txt

# Initialize the database

psql -U postgres -f sql/schema_ddl.sql


👤 Author
Omar El Tokhy

💼 LinkedIn: https://www.linkedin.com/in/omareltokhy1995/
🐙 GitHub: https://github.com/see0mar
📧 omareltokhey@gmail.com

Targeting: Data Analyst | Analytics Engineer | BI Developer roles.


Disclaimer: This project was completed during an internship with Saint Louis University. All PII (Personally Identifiable Information) has been masked or synthesized for confidentiality.
