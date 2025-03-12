# Bellabeat: How can a wellness company play it smart?

&nbsp;&nbsp;&nbsp;&nbsp;Bellabeat, a high-tech company that manufactures health-focused smart products.Sršen used her background as an artist to
develop beautifully designed technology that informs and inspires women around the world. Collecting data on activity, sleep,
stress, and reproductive health has allowed Bellabeat to empower women with knowledge about their own health and habits.
Since it was founded in 2013, Bellabeat has grown rapidly and quickly positioned itself as a tech-driven wellness company for women.

## Scenario

&nbsp;&nbsp;&nbsp;&nbsp;Bellabeat is a successful small company, but it has the potential to become a larger player in the
global smart device market.Urška Sršen, cofounder and Chief Creative Officer of Bellabeat, believes that analyzing smart
device fitness data could help unlock new growth opportunities for the company.As a Bellabeat marketing analytics team we have been
asked to focus on analyze smart device data to gain insight into how consumers are using their smart devices.The insights we discover
will then help guide marketing strategy for the company.

### ***This Business Task Have been go through six phase of data analysis process:***

### :question: [Ask](#question-ask-phase-identifying-the-business-task)
### :world_map: [Prepare](#world_map-prepare-phase-gathering-and-understanding-data-sources)
### :computer: [Process](#computer-process-phase-cleaning-and-transforming-data)
### :mag: [Analyze](#mag-analyze-phase-performing-calculations-and-identifying-trends)
### :chart: [Share](#chart-share-phase-creating-visuals-and-insights)
### :rocket: [Act](#rocket-act-phase-final-recommendations)



## :question: ASK PHASE: Identifying the Business Task
Bellabeat aims to leverage smart device usage data to gain insights into customer behavior and improve its marketing strategy. The analysis will answer:
1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers?
3. How could these trends help influence Bellabeat’s marketing strategy?

#### ***The Key Stakeholders :***

\- Urška Sršen-Chief Creative Officer and Bellabeat’s Co-founder

\-Sando Mur-Mathematician and Bellabeat’s Co-founder

\-Bellabeat’s marketing analytics team.

## :world_map: PREPARE PHASE: Gathering and Understanding Data Sources
- **Data Source**: [Fitbit Fitness Tracker Public Dataset from Kaggle](https://www.kaggle.com/datasets/arashnic/fitbit)
- **Data Overview**: This Kaggle data set contains personal fitness tracker from thirty fitbit users.
Thirty eligible Fitbit users consented to the submission of personal tracker data, including minute-level
output for physical activity, heart rate, and sleep monitoring. It includes information about daily activity,
steps, and heart rate that can be used to explore users’ habits. The data set consists of 11 csv files in total.
When we bring them together, we can access the date range 03.12-04.12 and 35 users.

### ROCCC Aproach:

- **Reliablity**: The source of the data tells us that the data was taken from thirty users. However, when we check
the data, we come across thirty-five users. This raises some doubts about the accuracy of the data.

- **Original**: Thirty eligible Fitbit users consented to the submission of personal tracker data, including
minute-level output for physical activity, heart rate, and sleep monitoring. This dataset generated by respondents
to a distributed survey via Amazon Mechanical Turk between 03.12.2016-04.12.2016.

- **Comprehensive**: When the data is examined, the fact that it does not include data such as age, gender, race,
  region of residence shows that the data is not comprehensive. This will cause certain limits and prevent us from
  reaching a certain level of insight.
  
- **Curent**: Data is from 03.12.2016 to 04.12.2016 so it is not current so some trends may have changed over time.
  
- **Cited**: Data has CC0: Public Domain, dataset made available through Möbius from a distributed survey via Amazon Mechanical Turk.
 
 :exclamation: ***Limitations***: Small sample size, possible bias, and lack of diversity.

 :arrow_up: [Back to the Top](#bellabeat-how-can-a-wellness-company-play-it-smart)



## :computer: PROCESS PHASE: Cleaning and Transforming Data
### SQL Data Cleaning
```sql
-- Checking for missing values
SELECT column_name, COUNT(*) AS missing_values
FROM FitBit_Data
WHERE column_name IS NULL
GROUP BY column_name;

-- Removing duplicate records
DELETE FROM FitBit_Data
WHERE rowid NOT IN (
    SELECT MIN(rowid)
    FROM FitBit_Data
    GROUP BY user_id, activity_date
);
```

### R Data Cleaning
```r
# Load necessary libraries
library(dplyr)
library(ggplot2)

# Load dataset
data <- read.csv("fitbit_data.csv")

# Checking for missing values
colSums(is.na(data))

# Removing duplicates
data_clean <- data %>% distinct()
```
:arrow_up: [Back to the Top](#bellabeat-how-can-a-wellness-company-play-it-smart)

## :mag: ANALYZE PHASE: Performing Calculations and Identifying Trends
### SQL Analysis
```sql
-- Aggregating daily activity data
SELECT user_id, activity_date, SUM(total_steps) AS daily_steps, SUM(total_calories) AS daily_calories
FROM FitBit_Data
GROUP BY user_id, activity_date;

-- Identifying user activity levels
SELECT user_id,
       AVG(total_steps) AS avg_daily_steps,
       CASE 
           WHEN AVG(total_steps) >= 10000 THEN 'Highly Active'
           WHEN AVG(total_steps) BETWEEN 5000 AND 9999 THEN 'Moderately Active'
           ELSE 'Sedentary'
       END AS activity_level
FROM FitBit_Data
GROUP BY user_id;
```

### R Analysis
```r
# Average steps per user
avg_steps <- data_clean %>%
  group_by(user_id) %>%
  summarise(avg_steps = mean(total_steps))

# Activity level classification
data_clean <- data_clean %>%
  mutate(activity_level = case_when(
    total_steps >= 10000 ~ "Highly Active",
    total_steps >= 5000 ~ "Moderately Active",
    TRUE ~ "Sedentary"
  ))
```
:arrow_up: [Back to the Top](#bellabeat-how-can-a-wellness-company-play-it-smart)

## :chart: SHARE PHASE: Creating Visuals and Insights
### Tableau Dashboard
- **Step Trends vs. Calories Burned**
- **Activity Levels Across Users**
- **Sleep Patterns and Device Usage**

### R Data Visualization
```r
# Steps vs. Calories Burned
ggplot(data_clean, aes(x = total_steps, y = total_calories)) +
  geom_point(color = "blue") +
  labs(title = "Steps vs Calories Burned")
```
:arrow_up: [Back to the Top](#bellabeat-how-can-a-wellness-company-play-it-smart)

## :rocket: ACT PHASE: Final Recommendations
1. **Target Audience Identification**: Focus marketing efforts on active users who track steps and calories frequently.
2. **Product Enhancement**: Improve sleep-tracking features based on observed sleep patterns.
3. **Strategic Marketing**: Personalize app recommendations based on user activity levels.

### GitHub Repository Structure
- `README.md`: Overview of the business task, approach, and findings.
- `SQL_Analysis.sql`: SQL queries used for data processing and analysis.
- `R_Analysis.R`: R script for data cleaning, transformation, and visualization.
- `Tableau_Dashboard.twb`: Interactive visualizations.

This structured approach ensures transparency and clarity in presenting Bellabeat’s smart device usage insights.

