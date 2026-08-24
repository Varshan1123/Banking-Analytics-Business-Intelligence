# Banking-Analytics-Business-Intelligence


**Step 1: Understand the Business Problem:**

I just started with few question:

- What problem is the bank facing?
- Why does the business need this dashboard?
- Who will use this dashboard?
- What decisions should management make from this report?


**Business Problem Statement:**

A bank processes thousands of customer transactions daily across multiple channels, including Mobile Banking, Internet Banking, ATMs, POS systems, and Branch Banking. While all transaction data is captured in the core banking system, the organization currently lacks an effective way to analyze and act upon this information efficiently.

**Problem 1 – Fragmented Reporting:**

Different departments rely on isolated, manual Excel reports. This creates inconsistencies and significantly slows down the reporting cycle.

**Problem 2 – Poor Financial Visibility:**

Leadership lacks real-time insights into daily financial performance.

It is difficult to quickly ascertain:

- today's transaction volume
- total deposits
- total withdrawals
- net cash flow.

**Problem 3 – Limited Customer Insights:**

Business units struggle to identify key customer trends, such as:

- the most utilized banking channels
- cities generating the highest transaction volumes
- customer occupations driving the most revenue

**Problem 4 – Risk Monitoring:**

The bank needs a more robust system to proactively flag on:

- high-value transactions
- identify multiple failed login attempts
- detect suspicious transaction patterns
- pinpoint the top ten highest-risk locations.

**Project Objectives & Target Audience:**

**Core Objective:** To design and deploy a centralized analytics solution that eliminates manual reporting delays and empowers banking teams to make faster, data-driven decisions.

**Target Audience:** This dashboard will serve cross-functional teams, including:

- Executive Management
- Finance and MIS Teams
- Banking Operations
- Risk & Fraud Teams
- Business and Data Analysts

**Business Value:**

Implementing this centralized analytics solution will deliver immediate strategic value to the organization by:

1. **Increasing Operational Efficiency:** Drastically reducing the time spent on manual data extraction and report preparation.
2. **Enhancing Financial Oversight:** Providing a single, centralized view to monitor core financial KPIs in real time.
3. **Improving Risk Mitigation:** Enabling faster identification and investigation of high-risk transactions and suspicious behavior.
4. **Driving Strategic Growth:** Tracking customer behavior and channel preferences to support highly targeted, data-driven business decisions.

**Step 2: Understand the Dataset**

- What does this column represent? / Why is it important? / How can it help answer a business question?

| column              | business question                                                                                                   |
| ------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Transaction_ID      | Helps calculate "Total Transaction Volume" (by counting the IDs)                                                    |
| Account_ID          | Helps track customer behavior and identify accounts with unusual activity.                                          |
| Transaction_Amount  | Answers "What is today's transaction value?", "High-value transactions", and calculates total deposits/withdrawals. |
| Transaction_Type    | answers "How much money was deposited?", "How much was withdrawn?", and "What is the net cash flow?".               |
| Transaction_Date    | Answers "What is today's transaction value?" and helps monitor transaction trends over time.                        |
| Account_Balance     | Helps Risk & Fraud teams identify suspicious patterns (e.g., large withdrawals leaving a near-zero balance).        |
| Customer_Age        | Helps track customer behavior (e.g., do younger customers use Mobile Banking more than ATMs?).                      |
| Customer_Occupation | Directly answers "Which occupations contribute the most business?".                                                 |
| Transaction_Channel | Directly answers "Which channel is most used?".                                                                     |
| Location            | Answers "Which city generates the highest transaction value?" and helps identify the "Top 10 High-risk locations".  |
| Login_Attempts      | Directly answers "Multiple login attempts" for the Risk Monitoring objective.                                       |

**Step 3: Validate the Data in Excel**

1. Duplicate Primary Key Check-> Ensure every **Transaction_ID** is unique using Conditional Formatting.
2. Missing / Blank Values Check -> using (go to special) ctrl+a & ctrl+g ->special->blank.
3. Data Type & Formatting Rules<br>
   For Transaction_Amount & Account_Balance **->** Change dropdown from _General_ to Number (set decimal places to 2).<br>
   For Transaction_Date **\->** Change dropdown to Custom (YYYY-MM-DD HH:MM)
4. Category Standardization (Spelling & Whitespace)<br>
   For Transaction_Type, Transaction_Channel, Location, Customer_Occupation -> Insert PivotTable and check spelling consistency error
5. Boundary & Logical Constraints (Data Validation Rules)<br>
   For Customer_Age -> Go to Data tab -> Data Validation -> Under Allow, choose Whole Number -> Set Data to Between Min: 18 | Max: 100<br>
   For Login_Attempts -> -> Go to Data tab -> Data Validation -> Under Allow, choose Whole Number -> Greater than or equal to 0.

**Step 4: Create the Database**

Create database BankingDB

**Step 5: Create the Table**

CREATE TABLE banking_transactions

(

transaction_id VARCHAR(15) PRIMARY KEY,

account_id VARCHAR(15),

transaction_amount NUMERIC(12,2),

transaction_type VARCHAR(30),

transaction_date DATETIME,

account_balance NUMERIC(12,2),

customer_age INT,

customer_occupation VARCHAR(50),

transaction_channel VARCHAR(30),

location VARCHAR(50),

login_attempts INT

);

**Step 6: Import the Dataset & verify**

SHOW VARIABLES LIKE "secure_file_priv";

LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/banking_Transactions_working.csv'

INTO TABLE banking_transactions

FIELDS TERMINATED BY ','

ENCLOSED BY '"'

LINES TERMINATED BY '\\r\\n'

IGNORE 1 LINES

(

transaction_id,

account_id,

transaction_amount,

transaction_type,

@var_date,

account_balance,

customer_age,

customer_occupation,

transaction_channel,

location,

login_attempts

)

SET transaction_date = STR_TO_DATE(@var_date, '%d-%m-%Y %H:%i');

Verify:

SELECT COUNT(\*) AS total_rows_imported FROM banking_transactions;

SELECT \* FROM banking_transactions LIMIT 10;

SELECT transaction_id, transaction_date, transaction_amount FROM banking_transactions LIMIT 10;

**Step 7: Perform SQL Business Analysis**

1: Total Transaction Volume

2: Total Transaction Dollar Value

3: Net Cash Inflow vs. Outflow (Deposit vs. Withdrawal)

4: Average Customer Account Balance

5: Top Banking Channels by Transaction Value

6: Geographical Transaction Concentration

7: Customer Occupation Revenue Contribution

8: High-Risk Account Flagging (Elevated Login Failures)

9: High-Risk Locations for Fraud Monitoring

10: Primary Transaction Channel by Demographic Age Group

11: Top VIP Customers by Balance and Spending

12: Low-Engagement & Churn Risk Accounts

13: Month-over-Month (MoM) Growth Trends

14: Distribution Across All Payment Types

15: Weekday vs. Weekend Average Transaction Size

16: Peak Server Load Analysis (Hourly Distribution)

**Step 8: Connect MySQL to Power BI**

Verify the connection

1. Row Count Verification
2. Primary KPI Spot Check (total Transaction amount)
3. Data Type & Schema Audit
4. Live Refresh Test (add a row to database and checked with powerbi)

**Step 9: Dashboard creation**

**Dashboard 1: Executive Overview Dashboard**

**Dashboard 2: Financial Dashboard**

**Dashboard 3:** **Risk Dashboard**

**Executive Overview Dashboard**

| **Dashboard Visual**                                                                                                                                               | **Primary Business Question Answered**                              | **Actionable Value**                                                                                                                                                                              |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **KPI Ribbon**<br><br>Total Transaction, Total Customer, Total Amount, Avg Amount, Avg Balance, High Risk Transaction<br><br>(Tnx_id where login >=3 / Tlt tnx_id) | What is our aggregate operational volume and risk exposure?         | Provides immediate context on total scale (Customers, Amount, Transactions). The High Risk Transaction % alerts compliance officers to potential systemic fraud issues requiring immediate audit. |
| **Monthly Transaction Trends**                                                                                                                                     | How does transaction count correlate with monetary value over time? | Reveals whether spikes in activity are driven by millions of small retail transactions or a few massive corporate transfers, guiding server load management and Treasury forecasting.             |
| **Transaction Type Distribution**                                                                                                                                  | What is the fundamental utility of our bank for customers?          | Breaks down exact usage (NEFT, UPI, Salary, POS). If UPI and IMPS are growing exponentially, the bank knows to prioritize digital server uptime over physical ATM maintenance.                    |
| **Transaction Channel Distribution**                                                                                                                               | Where is our customer traffic originating?                          | Highlights infrastructure dependencies. Even distribution across Mobile, Internet, and Branch channels indicates a need for omni-channel support and budget balancing.                            |
| **Top 10 Locations by Amount**                                                                                                                                     | Which regional markets drive the highest financial value?           | Informs geographic strategy. High-value regions like Lucknow and Jaipur can be targeted for wealth management campaigns or premium branch expansions.                                             |
| **Occupation Distribution**                                                                                                                                        | What is the demographic makeup of our transaction base?             | Drives marketing segmentation. Knowing that Engineers and Students hold equivalent shares allows for highly targeted loan or credit card product launches.                                        |
| **Security Exposure Risk Heatmap**                                                                                                                                 | Where are our highest localized security vulnerabilities?           | Allows regional risk managers to see if a specific channel in a specific city (e.g., ATMs in Ahmedabad) is experiencing abnormal activity, prompting targeted security crackdowns.                |
| **Recent Transaction Table**                                                                                                                                       | What exactly is happening at the ground level right now?            | Serves as an operational audit trail. Allows branch managers or support staff to spot-check specific Account IDs, transaction types, and timestamps without leaving the executive view.           |

**Financial Dashboard**

| **Dashboard Visual**                                                                                                                                                   | **Primary Business Question Answered**                       | **Actionable Value**                                                                                                                                          |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **KPI Ribbon**<br><br>Total Deposit,<br><br>Total Withdrawal<br><br>Net Cash Flow (T.D -T.W)<br><br>D/W Ratio (T.D/T.W)<br><br>Avg Acc Balance,<br><br>Avg Transaction | Is our baseline financial health stable today?               | Provides an instant health check. The QoQ (Quarter-over-Quarter) indicators warn executives immediately if withdrawals are accelerating faster than deposits. |
| **Monthly Liquidity Trend**                                                                                                                                            | Are there seasonal trends in our cash flow?                  | Helps the Treasury department anticipate months with heavy capital outflows (like holiday seasons or tax months) to manage investments accordingly.           |
| **Capital Allocation by Type**                                                                                                                                         | What financial activities drive our volume?                  | Identifies core product usage. If "Bill Payment" is growing, the bank can negotiate better vendor partnerships or introduce targeted fee structures.          |
| **Transaction Share by Channel**                                                                                                                                       | Are digital transformation efforts working?                  | Tracks the migration from expensive physical channels (Branch, ATM) to cost-effective digital channels (UPI, Mobile). Guides IT budget allocation.            |
| **Amount by Location**                                                                                                                                                 | Which geographic markets are outperforming?                  | Informs real estate and expansion strategy. High volume areas may need more ATMs or premium branches, while low volume areas might face consolidation.        |
| **Volume by Ticket Size**                                                                                                                                              | Is our infrastructure handling retail or enterprise traffic? | If micro-transactions (< 1K) dominate volume, IT must focus on server latency and uptime. If large transactions dominate, focus shifts to fraud prevention.   |
| **Avg Balance by Occupation**                                                                                                                                          | Who holds the most capital in our bank?                      | Directs the wealth management and marketing teams on where to focus acquisition campaigns and personalized premium services.                                  |
| **Hourly Velocity Heatmap**                                                                                                                                            | When do we face the highest operational stress?              | Allows branch managers and cash logistics teams to perfectly time physical cash deliveries and optimize employee shift schedules to match peak hours.         |

**Risk Dashboard**

| **Dashboard Visual**                                                                                                                                           | **Primary Business Question Answered**                                                                                                   | **Actionable Value**                                                                                                                                                                                                                                                                                                    |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **KPI Ribbon**<br><br>Composite Risk Score,<br><br>OFF-Hours Tnx Index,<br><br>Balance Drain %,<br><br>Irreversible Transfer Share %, Suspicious Transactions. | Are we currently under a severe attack, and what specific fraud tactics (like late-night transfers or full account drains) are trending? | If a specific gauge (e.g., _Balance Drain %_) breaches the designated threshold, leadership can instantly authorize emergency protocols, such as halting all high-value outbound transfers system-wide.                                                                                                                 |
| **Monthly Threat Exposure vs. Risk Score**                                                                                                                     | Is our financial risk increasing over time, and is our risk-scoring algorithm accurately tracking alongside actual monetary exposure?    | Enables proactive resource planning. If historical trends show massive spikes in exposure during specific months (like the surge from April to May), leadership can proactively increase server monitoring and fraud-team staffing ahead of those periods next year.                                                    |
| **Login Attempts Distribution**                                                                                                                                | Are bad actors attempting to brute-force their way into accounts, or is the traffic entirely normal, single-attempt logins?              | If the chart shows a sudden, thick tail forming on the right side (high volume of accounts with 6, 8, or 10+ failed attempts), the IT security team can immediately deploy CAPTCHAs, throttle IP addresses, or force password resets.                                                                                   |
| **Risk Tier Distribution**                                                                                                                                     | Out of our total active user base, exactly what proportion of accounts currently sits in the critical 'High Risk' zone?                  | Dictates team workload and capacity planning. If the "High Risk" (2K) or "Medium Risk" (12K) buckets expand rapidly, management knows exactly how many analysts need to be pulled from routine compliance tasks to handle active threat investigations.                                                                 |
| **Threat Vector Matrix: Location vs. Channel**                                                                                                                 | Geographically, where are the attacks concentrated, and which specific banking channels are they exploiting to extract the funds?        | Allows for highly targeted interventions. For example, if the matrix shows a dark red hotspot at the intersection of _Lucknow_ and _POS_, the bank can temporarily tighten security rules or lower transaction limits exclusively for POS purchases in Lucknow, without disrupting mobile or ATM users in other cities. |
| **High-Risk Account Watchlist**                                                                                                                                | Exactly which individual accounts are currently at the highest risk, and how much specific capital is exposed right now?                 | Direct remediation. Fraud analysts use this sorted list to freeze accounts, block pending transfers, and contact customers directly. Because it is sorted by _Suspicious Value at Risk_, analysts secure the largest monetary threats first                                                                             |
