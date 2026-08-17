# cleaning_data_python_
Employee Salary Data Cleaning & EDA

Project Overview

This project demonstrates a complete data cleaning and exploratory data
analysis workflow using Python and Pandas on an employee salary dataset.

The dataset contains 148,654 records and 13 columns before cleaning.
The notebook explores the structure and quality of the data, identifies
missing values and negative numeric values, standardizes numeric
columns, handles missing values, and visualizes potential outliers.

The original dataset is loaded from data_salaries.csv. The main fields
include employee name, job title, salary components, total compensation,
year, agency, and employment status.

Objectives

Explore the structure and characteristics of the dataset

Identify duplicate records and unnecessary columns

Inspect data types and missing values

Standardize salary-related numeric columns

Handle missing values

Identify negative values in numerical columns

Detect and handle outliers using the IQR method

Visualize numerical outliers using boxplots

Produce a cleaner dataset suitable for further analysis

Dataset

Original Dimensions

Metric                        Value

Rows                        148,654
Columns                          13
Source file     data_salaries.csv

Original Columns

Id

EmployeeName

JobTitle

BasePay

OvertimePay

OtherPay

Benefits

TotalPay

TotalPayBenefits

Year

Notes

Agency

Status

Technologies & Libraries

Python

Pandas

NumPy

Matplotlib

Seaborn

Missingno

Data Cleaning Workflow

1. Load the Dataset

The dataset is loaded with Pandas:

df = pd.read_csv('data_salaries.csv')

The initial dataset contains 148,654 rows and 13 columns.

2. Explore the Dataset

The notebook uses:

df.head()
df.tail()
df.describe()
df.info()

This provides an overview of the dataset, statistical summaries, data
types, and missing values.

3. Check Duplicates

Duplicate records are checked using:

df.duplicated().sum()

The notebook found no duplicated rows.

4. Remove Unnecessary Columns

The Id and Notes columns are removed:

df = df.drop(['Id', 'Notes'], axis=1)

This reduces the dataset from 13 to 11 columns.

5. Standardize Salary Columns

Several salary columns contain the text value Not Provided. These
values are replaced with 0 and converted to numeric types.

Example:

df['BasePay'] = df['BasePay'].replace('Not Provided', 0)
df['BasePay'] = df['BasePay'].astype(float)

The same transformation is applied to:

OvertimePay

OtherPay

Benefits

After cleaning, the salary fields are represented as numeric values.

6. Inspect the Year Column

The dataset contains four years:

2011
2012
2013
2014

The notebook converts the Year column to a datetime type.

7. Missing Value Analysis

Missing values are checked with:

df.isnull().sum()

Before missing-value treatment, the main missing-value counts included:

Column         Missing Values

BasePay                 605
Benefits             36,159
Status              110,535
Notes               148,654

Notes is removed as part of the cleaning process.

8. Handle Missing Values

The notebook applies different strategies depending on the column:

df['BasePay'] = df['BasePay'].fillna(0)

df['Benefits'] = df['Benefits'].fillna(
    df['Benefits'].mean()
)

df['Status'] = df['Status'].fillna('bfill')

After these transformations, the notebook checks the remaining missing
values.

Note: The Status operation above fills missing values with the
literal text "bfill"; it is not the Pandas backfill operation. This
README documents the notebook as implemented.

9. Separate Categorical and Numerical Columns

Categorical columns are selected with:

cat_cols = df.select_dtypes(include='str')

Numerical columns are selected with:

num_cols = df.select_dtypes(include=['int', 'float'])

This separation makes it easier to apply appropriate data-quality
checks.

10. Check Negative Values

Negative values in numerical columns are investigated using:

num_cols[num_cols < 0].count()

The notebook identifies negative values in salary-related columns.

The numerical data is then converted to absolute values:

num_cols = num_cols.abs()

A second check confirms that negative values are no longer present.

Outlier Detection

Boxplot Visualization

Potential outliers are visualized with a boxplot:

plt.Figure(figsize=(16,8))
sns.boxplot(data=num_cols)
plt.title('Boxplot of Numerical columns')
plt.xticks(rotation=45)
plt.show()

IQR Method

The notebook uses the Interquartile Range (IQR) method.

The calculation is:

Q1 = num_cols[col].quantile(0.25)
Q3 = num_cols[col].quantile(0.75)

IQR = Q3 - Q1

lower_bound = Q1 - 1.2 * IQR
upper_bound = Q3 + 1.2 * IQR

Rows outside these bounds are removed:

num_cols = num_cols[
    (num_cols[col] >= lower_bound) &
    (num_cols[col] <= upper_bound)
]

The project uses a 1.2 × IQR multiplier, as implemented in the
notebook.

Project Structure

employee-salary-data-cleaning/
│
├── data_cleaning.ipynb
├── data_salaries.csv
└── README.md

Key Skills Demonstrated

Data loading with Pandas

Exploratory Data Analysis (EDA)

Data profiling with info() and describe()

Duplicate detection

Column selection and removal

Data type conversion

Missing-value analysis and treatment

Numeric data standardization

Negative-value detection

IQR-based outlier detection

Boxplot visualization

Data quality validation

Future Improvements

Preserve the raw dataset and create a separate cleaned dataset

Use a boolean mask when applying IQR filtering across multiple
columns

Validate whether negative salary values represent legitimate
business cases before converting them with abs()

Convert the Year column using a year-aware datetime approach

Use a documented business rule for missing Status values

Add before/after data-quality metrics

Export the final cleaned dataset for downstream analysis

Author

Khalil Badharis

Data Analyst | Data Engineer | BI Analyst
