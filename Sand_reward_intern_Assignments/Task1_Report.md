## Task 1: Data Cleaning Report

### Tools Used
For this task, I opted to use Python to clean the dataset. Python is ideal for automating repetitive data-cleaning tasks and handling large datasets efficiently. I utilized the following libraries:
1. **Pandas** for data manipulation and cleaning.
2. **Openpyxl** for reading and writing Excel files.
3. **Plotly** to generate visual insights from the data.

### Data Wrangling Process

#### Loading the Data
I started by loading the provided Excel dataset using the `read_excel()` function in Pandas, specifying the correct sheet, "Mentorship_Sessions," to access the data.

```python
mentorship_session_df = pd.read_excel("Mentorship_Sessions.xlsx", sheet_name='Mentorship_Sessions')
```

#### Initial Data Overview
To understand the structure and quality of the dataset, I used `df.info()` and `df.describe()` to get an overview of the data types and check for any missing or inconsistent values.

```python
mentorship_session_df.info()
```
This revealed that the dataset consisted of 109 rows and 8 columns, with some missing values in key columns such as `Mentor_ID`, `Session_Number`, `Job_Info_Completed`, `Session_Date`, `Mentee_Name`, and `Session_Duration_Min`.

---

### Data Cleaning Steps

#### 1. Removing Null Values
Several rows contained missing values in important columns. Since these columns were critical for point allocation and analysis, I opted to drop rows with null values using the following command:

```python
mentorship_session_df.dropna(inplace=True)
```
This reduced the dataset to 106 rows.

#### 2. Removing Duplicates
Next, I checked for duplicate rows in the dataset using `df.duplicated()`. I identified and removed 2 duplicate rows, leaving the dataset with 104 rows.

```python
mentorship_session_df.drop_duplicates(inplace=True)
```

#### 3. Formatting Data Types
Some columns had inconsistent data types. For instance, the `Session_Date` column was stored as an object, so I converted it to a proper `datetime` format. I also ensured that numeric columns like `Session_Number` and `Session_Duration_Min` were correctly formatted as integers.

```python
mentorship_session_df['Session_Date'] = pd.to_datetime(mentorship_session_df['Session_Date'])
mentorship_session_df['Session_Number'] = mentorship_session_df['Session_Number'].astype(int)
mentorship_session_df['Session_Duration_Min'] = mentorship_session_df['Session_Duration_Min'].astype(int)
```

---

### Overview of the Cleaned Data
After completing the cleaning process, I performed another check on the dataset using `df.info()` to confirm the structure of the cleaned data. The cleaned dataset had 104 rows and 7 columns with no missing or duplicate entries:

```python
<class 'pandas.core.frame.DataFrame'>
Index: 104 entries
Data columns (total 7 columns):
 0   Mentor_ID             104 non-null    int64
 1   Mentor_Name           104 non-null    object
 2   Mentee_Name           104 non-null    object
 3   Session_Number        104 non-null    int64
 4   Session_Duration_Min  104 non-null    int64
 5   Job_Info_Completed    104 non-null    object
 6   Session_Date          104 non-null    datetime64[ns]
```

Additionally, I identified 5 unique mentors and 5 unique mentees in the dataset. Here is a summary of the cleaned data:
- **Mentor Emily Davis** held the most sessions (27), followed by Michael Thompson (24), David Kim (20), James Taylor (18), and Sarah Johnson (17).

#### Mentor-Mentee Distribution
![Mentor-Mentee Distribution](<Mentor-Mentee Distribution-1.png>)

#### Job Information Provided
Out of 104 sessions, 48 mentees provided job information during their sessions, while 57 did not. This is illustrated in the graph below:

![Job Info Provided](<Job Info Provided.png>)

---

### Key Findings
- The original dataset had **109 rows** and **8 columns**, with missing values in several critical columns.
- After cleaning, the dataset was reduced to **104 rows**.
    - **1 null** entry was found in each of the columns `Mentor_ID`, `Session_Number`, `Job_Info_Completed`, `Session_Date`, and **2 nulls** in `Mentee_Name` and `Session_Duration_Min`.
    - All missing and duplicate rows were removed for cleaner analysis.
- The dataset includes **5 unique mentors** and **5 unique mentees**.
- The cleaned data now allows for accurate point allocation and insights for further tasks in the assignment.

### Conclusion
The cleaned dataset is now ready for further analysis, including point allocation and insights generation, as required in Task 2 and Task 3. All steps in the data cleaning process were executed using Python, ensuring a streamlined and automated approach to data wrangling.

---

