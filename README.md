# Data Analyst Home Assignment: URL Security Analysis

Hello! 👋
Welcome to my submission for the Data Analyst home assignment. The goal of this project was to analyze a dataset of URL messages to identify infrastructure bottlenecks, quality issues, and service anomalies.

I used **Python (Pandas)** for the heavy lifting (cleaning and merging) and **Tableau** for the final visualizations.

🛠️ Tools & Environment
*   **Anaconda Navigator:** Used to manage my environment and launch Jupyter.
*   **Jupyter Notebook:** My main workspace for coding.
*   **Python (Pandas, JSON):** Used for data wrangling.
*   **Tableau Public:** Used for creating the interactive dashboard.


**Note on SQL:** Although the assignment mentioned SQL as an option, I decided to perform the analysis entirely in **Python (Pandas)**. Since I was recommended to use Jupyter Notebooks for this task, I felt much more comfortable and efficient using standard Pandas techniques to clean and manipulate the data rather than trying to force SQL queries into the notebook environment.


## 🧠 My Thought Process & Approach

### 1. Handling the Data
When I first opened the dataset, I saw four separate JSON files (`messages`, `priority`, `reputations`, etc.). Looking at them individually was confusing, and I instantly knew I needed to see the "whole picture."
*   **Action Plan:** My first step was to merge all four files on the `id` column. This ensured I didn't lose any data even if a URL was missing some attributes.

### 2. Data Cleaning & Challenges
I encountered a few interesting challenges while cleaning the data:
*   **The Redirects Column:** This column was tricky because it contained a mix of boolean values and lists. When I first tried to split the text to extract URLs, I got an error because of `Null` values.
    * For this, I converted the column to a string, cleaned the brackets, and carefully handled the nulls before splitting them into `has_redirect` and `url_redirects`.
*   **Date Anomalies:** During the analysis, I also noticed timestamps dating back to **1936**. This was clearly a data quality error, so I filtered these out during my visualization to focus on the relevant data (2020–2024).

### 3. Feature Engineering
I had to standardize the reputation scores to figure out exactly which sources are performing well and which are not.
*   **Logic:** I created a dictionary to map the various labels.
    *   `good` and `neutral` ➔ **Non-Malicious** (I treated Neutral as safe since it's not an active threat).
    *   `bad`, `phishing`, `malware` ➔ **Malicious**.
*   **Result:** This allowed me to create a binary `is_accurate` column to calculate which source makes the most mistakes.

---

## 📊 Key Findings & Insights

You can view my Dashboard here: [**View Live Dashboard**](https://public.tableau.com/views/DA_Assignment_Naveen/MyMain?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

### 1. Performance Bottlenecks
*   **Service_4 is the MVP:** It is the fastest service (lowest load time) and handles redirects almost 3 times less frequently than the others.
*   **Service_1 needs attention:** It is currently the slowest service and deals with a higher volume of "Failure" response codes compared to Service 4.

### 2. Data Quality Issues
*   **Source_4 is the most reliable:** It has the highest accuracy rate regarding URL reputations. Source_4 generates the least incorrect reputations compared to Sources 1, 2, and 3.
*   **Missing Data:** A significant number of records (~10,000) have a `Null` source. This is a pipeline failure that needs to be addressed, as we are ingesting data without knowing where it came from.

### 3. Finding Correlation in Page Size
*   I tried to analyze to see if "Page Size" was causing the slow load times, perhaps to find a root cause for the longer load time. Surprisingly, the data does not show any correlations. To me, it confirmed that the slowness in Service_1 is not because of the size of the files it is downloading.

---

## 📂 Repository Files
*   `NordSecAssign_DA_Naveen.ipynb`: The Jupyter Notebook containing all my code, notes, and cleaning steps.
*   `Final_Dashboard.png`: A static preview of the final dashboard.
*   `home_assignment_final.xlsx`: The processed dataset used for Tableau.
