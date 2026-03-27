# Automobile Repair Business Analytics - Fabric Project

## 📋 Project Overview

This Fabric project implements a comprehensive data analytics solution for a multi-location vehicle repair business. It tracks repair timelines, estimates, revenue, staff performance, and customer feedback across stores to analyze operational efficiency, financial performance, and customer experience.

**Data Architecture:** Bronze → Silver → Gold (Medallion Architecture)  
**Orchestration:** Monthly scheduled pipeline execution via GitHub source  
**Ingestion Method:** HTTP-based ingestion from GitHub  
**Last Updated:** March 27, 2026

---

## 🎯 Key Performance Indicators (KPIs)

The project tracks **11 primary KPIs** across operational and financial dimensions:

| KPI | Description | Frequency | Owner |
|-----|-------------|-----------|-------|
| **MTD Performance vs Previous MTD** | Month-to-date revenue and completed orders by store and manager | Monthly | Store Manager |
| **Average Days in Shop** | Average vehicle duration by store and service type | Monthly | Operations |
| **Survey Coverage** | Surveys sent vs. responded ratio by store | Monthly | Customer Service |
| **Survey Scores Summary** | Overall satisfaction metrics and store rankings | Monthly | Quality Assurance |
| **Revenue vs Budget** | Monthly revenue vs. budget by manager with achievement ranking | Monthly | Finance |
| **Top Technicians by Completion Time Accuracy** | Top 5 technicians meeting promised completion dates per store | Monthly | Operations |
| **Year-to-Date Revenue Growth** | YTD revenue vs. previous year with store rankings | Monthly | Finance |
| **Stage-wise Day Cycle Time** | Average days per stage (in → start → completion → delivery) | Monthly | Operations |
| **Estimator Accuracy** | Initial vs. final estimate accuracy by estimator | Monthly | Quality Assurance |
| **Technician Workload** | Orders handled and total days per technician per month | Monthly | HR/Operations |
| **Revenue vs Budget Manager Ranking** | Performance ranking of managers by budget achievement | Monthly | Finance |

---

## 📊 Data Model

### Source Tables (6 CSV Files)

#### 1. **order.csv** (Core Transaction Table)
**Primary Key:** `order_id`  
**Foreign Keys:** `store_id`, `technician_id`, `customer_id`, `estimator_id`

| Column | Data Type | Description |
|--------|-----------|-------------|
| order_id | String | Unique order identifier |
| store_id | String | Reference to store location |
| technician_id | String | Assigned repair technician |
| technician_name | String | Technician full name |
| service_type | String | Type of service (e.g., Oil Change, Major Repair) |
| vehicle_no | String | Vehicle registration number |
| vehicle_make | String | Vehicle manufacturer |
| vehicle_model | String | Vehicle model name |
| vehicle_in_datetime | Timestamp | When vehicle arrived at shop |
| vehicle_out_datetime | Timestamp | When vehicle left shop |
| planned_work_start_datetime | Timestamp | Estimated work start date/time |
| actual_work_start_datetime | Timestamp | Actual work start date/time |
| planned_completion_datetime | Timestamp | Estimated completion date/time |
| actual_completion_datetime | Timestamp | Actual completion date/time |
| promised_delivery_datetime | Timestamp | Promised delivery to customer |
| actual_delivery_datetime | Timestamp | Actual delivery to customer |
| order_status | String | Current status (Open, Completed, Delivered) |
| customer_name | String | Customer full name |
| customer_phone | String | Customer contact number |

#### 2. **estimate.csv** (Estimate Tracking)
**Primary Key:** `estimate_id`  
**Foreign Key:** `order_id`

| Column | Data Type | Description |
|--------|-----------|-------------|
| estimate_id | String | Unique estimate identifier |
| order_id | String | Reference to order |
| version_no | Integer | Estimate version (1, 2, 3...) |
| estimate_amount | Decimal | Estimated repair cost |
| created_at | Timestamp | Estimate creation date/time |
| created_by | String | Estimator who created estimate |
| estimate_type | String | Type (Initial, Revised) |
| estimator_id | String | Estimator identifier |
| estimator_name | String | Estimator full name |

#### 3. **invoice.csv** (Financial Transaction)
**Primary Key:** `invoice_id`  
**Foreign Key:** `order_id`

| Column | Data Type | Description |
|--------|-----------|-------------|
| invoice_id | String | Unique invoice identifier |
| order_id | String | Reference to order |
| invoice_date | Date | Invoice generation date |
| invoice_amount | Decimal | Final billed amount |
| payment_mode | String | Payment method (Cash, Card, Check) |
| currency | String | Currency code (USD, EUR, etc.) |

#### 4. **customer_survey.csv** (Customer Feedback)
**Primary Key:** `survey_id`  
**Foreign Key:** `order_id`

| Column | Data Type | Description |
|--------|-----------|-------------|
| survey_id | String | Unique survey identifier |
| order_id | String | Reference to order |
| survey_sent_date | Date | Date survey was sent to customer |
| survey_response_date | Date | Date customer responded |
| responded_flag | Boolean | Whether customer responded (Yes/No) |
| delivered_on_time_rating | Integer | Rating 1-5 for on-time delivery |
| work_quality_rating | Integer | Rating 1-5 for repair quality |
| cleanliness_rating | Integer | Rating 1-5 for shop cleanliness |
| communication_rating | Integer | Rating 1-5 for staff communication |
| overall_satisfaction_rating | Integer | Overall satisfaction rating 1-5 |

#### 5. **store.csv** (Location Dimension)
**Primary Key:** `store_id`

| Column | Data Type | Description |
|--------|-----------|-------------|
| store_id | String | Unique store identifier |
| store_name | String | Store location name |
| city | String | City where store is located |
| state | String | State/Province |
| manager_id | String | Store manager identifier |
| manager_name | String | Store manager full name |
| opened_year | Integer | Year store opened |
| store_type | String | Type of store (Main, Branch, Franchise) |

#### 6. **ns_budget.csv** (Budget Planning)
**Primary Key:** Composite (`ns_store_id`, `month`)

| Column | Data Type | Description |
|--------|-----------|-------------|
| ns_store_id | String | Store identifier for budget |
| month | Date | Budget month (YYYY-MM-01 format) |
| budget_amount | Decimal | Monthly budget allocation |
| approved_by | String | Manager who approved budget |

---

## 🏗️ Project Structure
