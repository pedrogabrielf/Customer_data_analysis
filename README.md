# README - Data Science Project: Salesforce Data Analysis

# Pedro Gabriel Fonseca
# Curitiba, Paraná, Brazil
# (41) 99129-2213
# E-mail: pedrofons8@gmail.com


## Overview

The project aims to analyze data extracted from Salesforce, processing and transforming the data using Python and SQL to generate valuable insights that can support business decisions. This README outlines the steps taken in parts 1, 2, 3, and 4 of the project, focusing on exploratory analysis, data transformation, visualizations, and insights for the company.

## The project was divided into the following parts:

### Part 1: Data Exploration
	• I load and explore the data from the two provided datasets (df_accounts and df_support_cases), to understand their structure, content and identify inconsistencies, null values ​​and other important characteristics.

### Part 2: Data Processing
	• Use Python and SQL to join datasets and derive meaningful business metrics. Emphasis was placed on using SQL for aggregations and transformations.

### Part 3: Data Visualization
	• Created visualizations using Python libraries such as Matplotlib, Seaborn and Plotly, based on the defined KPIs, to present valuable insights and facilitate data interpretation.

### Part 4: Business Insights
	• Derive key insights from data and propose actionable recommendations that can be applied in a business context.

# Part 1: Data Exploration  - df_accounts
		• I used the main functions for general analysis such as .head() .info() .sum() .isnull() duplicated() describe() value_counts()
		• Total rows: 1,415
		• Total columns: 5 (after creating some auxiliary columns for analysis, we kept the original columns intact).
		• Original columns:
			1.	account_sfid
			2.	account_name
			3.	account_created_date
			4.	account_country
			5.	account_industry

	## Analysis by Column
	### account_sfid
		• Description: Unique identifier for each account.
		• We checked for duplicates (.duplicated().sum()) and null values ​​(.isnull().sum()): both returned 0, confirming that all IDs are unique and there are no missing values.
		• All values ​​are 64 characters long, indicating standardization by the source system (Salesforce).

	Conclusion: No changes needed. The column is intact and consistent.

	### account_name
		• Description: Descriptive name of the account.
		• We checked the number of unique names and null values: 1,414 unique names for 1,415 records (there is 1 duplicate). No null values.
		• The format of the names follows a "Customer_<ID>" pattern, all with 17 characters.
		• We decided not to change the duplicate name, since account_sfid is the primary key and remains unique.

	Conclusion: Column without critical problems. Duplication documented, but kept as is.

	### account_created_date
		• Description: Account creation date.
		• Converted to datetime (pd.to_datetime()).
		• We created 3 columns to separate, year, month, day of the week.
		• There are no null values.
		• We found that dates range from 2007 to 2025, with only 1 record in 2025 (possible test data or future input).
		• We identified high account creation in some years and months (e.g. peaks in November).

	Conclusion: Conversion to datetime was sufficient.

	### account_country
		• Description: Country where the account was registered.
		• We found 7 null values.
		• There are 72 different countries.
		• Main countries: United States (553), China (73), Canada (68), etc.

	Conclusion: We decided to fill (fillna("Unknown")) the 7 nulls with "Unknown" to maintain consistency and not lose records.

	### account_industry
		• Description: Industry or segment to which the account belongs.
		• We found 13 null values.
		• Existence of 22 different industries.
		• Main: Pharmaceuticals (421), Printing (265), Packaging and Containers (252), etc.

	Conclusion: We filled the 13 null values ​​with "Unknown".

	### Transformations Performed

	Handling Null Values
		• account_country: We replaced 7 null values ​​with "Unknown".
		• account_industry: We replaced 13 null values ​​with "Unknown".

	Data Type Conversion
		•	account_created_date: Converted from object to datetime64[ns].


# Parte 1: Data Exploration  - df_support_cases

	Using the df_support_cases.info() command, we can see:
		• Total Columns: 16 (e.g.: case_sfid, account_sfid, case_number, case_created_date, etc.)
		• Total Records: 10k

Main Columns
	1. case_sfid - Unique identifier for the case.
	2. account_sfid - Associated account identifier.
	3. case_number - Case number, used for reference.
	4. case_created_date - Date the case was created.
	5. case_closed_date - Date the case was closed, if any.
	6. case_status, case_priority, case_severity, case_reason, case_type, case_category - Metadata describing the nature and urgency of the case.

# Analysis Performed
	1. Checking for Null Values
		• Using df_support_cases.isnull().sum(), we identified:
		• account_sfid: 1593 null values.
		• case_closed_date: 942 null values.
		• Other columns: 0 null values.
	2. Handling Null account_sfid
		• We decided to standardize cases without a linked account with the value "Unknown".

# Frequency of Values ​​(value_counts)
	• For most categorical columns (e.g. case_status, case_priority, case_reason, case_type, case_category)

# Additional Notes
	• case_closed_date Null: We interpret this as “open case”. We chose to keep these values ​​null, since there is no closing date
	• Consistency with the Accounts Dataset: The use of account_sfid as a key (even though some rows are now marked as "Unknown") will be crucial in the integration (JOIN) with the accounts dataset in the next phase of the project.
	• Future Metrics Analysis: With dates in datetime format, it will be possible to calculate the average resolution time (for closed cases) and track case opening trends by period.

# Parte 2: Data Processing
Insights:

	1.  Number of Cases by Country
		- Discover in which countries support is most demanded. This helps the business understand where there is the greatest need for service or possible recurring problems by region.

	2. Number of Cases by Industry
		- Reveals which sectors require the most support. Useful for prioritizing specialized support teams or developing specific solutions for certain industries.

	3. Number of Cases per Account (TOP 5)
		- Identifies the 5 most active accounts in support. These accounts can be important customers or customers with a lot of problems. This guides the support team in planning proactive actions.

	4. Average Resolution Time per Account
		- Measures in days how long it takes, on average, to resolve cases for each account. Allows you to compare:
		- Accounts with high resolution times, which may be unsatisfied.

	5. Distribution of Cases by Severity
		- Allows you to know the predominant level of severity in cases, helping the support team to scale efforts (e.g., if the majority are of high severity, more resource allocation is needed).

	6. Distribution of Cases by Priority
		- Similar to severity, it identifies how urgent most cases are. If many cases are “High Priority”, the company may need to review triage processes.

	7. Resolution time per country
		- Insight: Helps understand regional variations in resolution time. There may be language barriers, time zones, or other factors in certain countries.

# Parte 3: Data Visualization

## Section 1 - Overview (Generic Charts)
	1.1 – Number of Cases by Country
	1.2 – Case Distribution by Country (Pie Chart – TOP 5 + OTHERS)
	1.3 – Account Distribution by Country
	1.4 – Number of Accounts by Country

## Section 2 – Severity and Priority Analysis
	2.1 – Distribution of Cases by Severity
	2.3 – Distribution of Cases by Priority
	2.4 – Distribution of Serious Cases by Country (USA)
	2.5 – Distribution of Serious Cases by Industry in the USA

## Section 3 - Specific Analysis of Industries and Type of Calls
	3.1 – Industries Related to the Top 3 Countries
	3.2 – Number of Cases by Industry
	3.3 – Distribution Cases by Type and Severity (Top 10 Types)
	3.4 – Distribution Cases by Type and Severity (Top 3 Types)
	3.5 – Average Resolution Time by Industry
		3.5.1 – Average Resolution Time per year
	3.6 – Average Resolution Time of Cases in the USA by Year of Account Creation
	3.7 – Countries with the Longest Resolution Time
		3.7.1 – Case Distribution by Severity in Countries with the Longest Resolution Time
	3.8 – Top 10 Accounts with the Longest Average Resolution Time
	3.9 – Evolution of Accounts Created by Month and Year
	3.10 – Number of Cases by Accounts (Top 5)

## Section 4 - Performance and Trend Analysis
	4.1 – Resolution Time Trend (Average, Maximum, Minimum)
	4.2 – Severity by Cases
	4.3 – Distribution of Cases by Country for the Top 3 Industries
	4.4 – Average, Maximum, and Minimum Resolutions of US Cases with 5 Most Present Industries
	4.5 – Account Creation Trend until 2030

## Section 5 - Analysis by countries with the most accounts (USA, CHINA, CANADA)
	5.1 - USA
	5.2 - CHINA
	5.3 - CANADA

# Part 4: Business Insights
	Based on your findings, answer the following questions:

	1. What are the key insights you derived from the data and visualizations?

		1.1 - USA has the highest demand for support, followed by China and Canada. 

		1.2 - The table below quickly shows the average support resolution time by case type in the US

		| Case Type  | Average time |
		| ------------- | ------------- |
		| License Activation  | +2 days  |
		| Software Performance  | +10 days  |
		| User Access issue | +4 days |

		1.3 - Top 3 Industries with the most cases. Pharmaceuticals, Information Technology and Printing. The table below shows the average resolution time for each case.

		| Industry  | Average time |
		| ------------- | ------------- |
		| Farmaceutica  | +6 days  |
		| Information Technology  | +-1 day  |
		| Printing | +5 days |

		1.4 - Distribution of Severe Cases: Severe cases are more prevalent in the USA, especially in the pharmaceutical industry.

		1.5 - Through the graphs that were generated, it can be seen that the United States is the leader in serious cases, especially in the pharmaceutical industry, which also has an average resolution time of more than 6 days, which is absurd and needs to be improved.

		1.6 - I was able to observe an inconsistency in solving technology-related problems. Canada practically has cases related to the technology industry that have an excellent average resolution time (Approximately 1 day), however, the United States has two problems related to technology, "Software Performace" and "User Access Issue", respectively having + 10 days and +4 days, as these are technology-related problems, they should be resolved faster!

		1.7 - Canada has a serious problem in documenting and specifying the type of case being analyzed, there is a lack of data to conclude better data.



	2. Propose two actionable recommendations that the business could take based on these insights.

		2.1 Allocate More Support Resources to the US: Case demand in the US is significantly higher, which may justify increasing support staff or automating processes. 40.2% of cases are from the USA, and with a delay of +10 days for Software Performance issues and +4 days for User Access Issues, it is clear that the IT team urgently needs to be revised to reduce this time considerably. The pharmaceutical industry represents 42.2% of cases per industry in the United States, and has an average resolution time of +6 days, which demonstrates once again that the USA has a serious problem with resolution time for open supports.

		2.2 The company must prioritize collecting complete and consistent data for Canada, seeking to fill any gaps in information such as case details, affected sectors and resolution times. One way to do this is to better integrate Canadian support systems with data analytics platforms, ensuring all interactions and metrics are captured correctly. With more data available, it will be possible to analyze further and identify country-specific patterns.


