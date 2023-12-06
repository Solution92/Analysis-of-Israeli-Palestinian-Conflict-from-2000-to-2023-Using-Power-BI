# Analysis-of-Israeli-Palestinian-Conflict-from-2000-to-2023-Using-Power-BI
This Dataset records fatalities in the Israel-Palestinian War for 23 years.

## Tables of Content
- [Problem Statement](#problem-statement)
- [Project Objective](#project-objective)
- [Data Source](#data-source)
- [Tools Used](#tools-used)
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)
- [Data Cleaning With PostgreSQL](#data-cleaning-with-postgresql)
- [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
- [Data Analysis](#data-analysis)
- [Individual Visuals](#individual-visuals)
- [The Dashboard](#the-dashboard)
- [Result and Findings](#result-and-findings)
- [Recommendation](#recommendation)
- [Limitation](#limitation)
- [Reference](#reference)


### Problem Statement

After the October 7 attack, the Israeli cabinet formally declared war against Hamas, followed by a directive from the defense minister to the Israeli Defense Forces (IDF) to carry out a “complete siege” of Gaza.

Remember, early this month, October 2023, war broke out between Israel and Hamas, the militant Islamist group that has controlled Gaza since 2006. Hamas fighters fired rockets into Israel and stormed southern Israeli cities and towns across the border of the Gaza Strip, killing and injuring hundreds of soldiers and civilians and taking dozens of hostages. The attack took Israel by surprise, though the state quickly mounted a deadly retaliatory operation.

As a Data Analyst, the foregoing is the reason why my Analytical mind was prompted and many questions arose. What is causing these conflicts now and then?

This led to my research on the Dataset on Kaggle and in this Analysis, I want to show us the exploration Process, the analytical methods/or techniques used, and the final findings of the Israel/Palestinian Conflict for 23 years or more.

### Project Objective

From the problem statement of this dataset, my Analysis is aimed at:

- Exploring the Dataset and tracking the trends in fatalities over time to identify any significant changes, spikes, or declines in the number of fatalities.

- To conduct a demographic analysis by examining the age, gender, and citizenship of the individuals killed. Determine if there are any notable patterns or disparities in the data.

- I will use the event location, district, and region information to perform geospatial analysis. I will also Visualize the distribution of fatalities on a map and identify areas that have experienced higher levels of violence.

- I want to investigate the extent of individuals’ participation in hostilities before their deaths. Will also analyze the relationship between participation and the circumstances surrounding each fatality.

- I will, however, examine the types of injury inflicted on individuals. Identify the most common types of injuries and assess their severity.

- I want to analyze the ammunition and means by which the individuals were killed. Will determine the most frequently used weapons or methods and evaluate their impact.

- Finally, I want to create a profile for victims to show their age, gender, citizenship, and place of residence. Identify common characteristics among the victims.

This Analysis will help the Israeli /Palestinian neighborhood and the entire world to see the effects of such conflicts on the economy and the world at large.

### Data Source

You can download the dataset used for this analysis from [Here](https://www.kaggle.com/datasets/willianoliveiragibin/fatalities-in-the-israeli-palestinian)

### Tools Used

- PostgreSQL/Excel — Data Cleaning, Transformation, and Analysis

- Power BI — Used For Creating Reports.

### Data Cleaning and Preparation

The Dataset has 17 Columns and over 1000 Rows and I enthusiastically wanted to: Explore and Analyze Fatality Trends, Demographic Analysis, Geospatial Analysis, Hostilities Participation Analysis, Injury Analysis, Analysis of Weapons used, and finally I will create Victims’ Profiles.

![Screenshot_133](https://github.com/Solution92/Analysis-of-Israeli-Palestinian-Conflict-from-2000-to-2023-Using-Power-BI/assets/144762124/5d09b007-1257-4859-b8f3-a6ec06bf003c)
Raw Dataset

The Dataset has 17 Columns with these attributes viz:

- Name: Variable characters to indicate the Victims’ name. That is the number of people who were killed or injured during the conflict or battle.
- Date of Event: An integer representing the day, month, and year of the conflict.
- Age: It’s a Positive Integer showing how old the victims are or were on or before the conflict.
- Citizenship: Variable characters which indicate the victim's citizenship.
- Event Location: Variable characters also show the particular location of the victims at the time of the conflict.
- Event Location District: Variable characters that represent the victims’ district at the time of the conflict.
- Event Location Region: Variable characters showing the region of the victims at the time of the conflict.
- Date of Death: An integer representing the day, month, and year of the victim's death.
- Gender: String variable of the victims’ gender.
- Took Part in the Hostilities: A set of variable characters represented by simple ‘Yes’ or ‘No’, to indicate if the victim took part in the conflict before his/her death.
- Place of Residence: Variable characters showing where Victim lived before his/her death.
- Place of Residence District: Variable characters showing the particular district the Victim lived in before his/her death.
- Types of Injury: Variable characters indicating the type of injury that led to the death of the victim.
- Ammunition: Variable characters to indicate the type of ammunition that killed the victim.
- Killed by: Variable characters showing the country's security forces that killed the victim.
- **Notes:** Variable characters showing how, what, who, when, and where the victim was killed. It indicates a summary of the attributes of the victim’s death.

### Data Cleaning With PostgreSQL

This is the process of standardizing the Dataset before creating a report. It starts with understanding the Dataset. Removing or replacing ‘null’ values and ‘blank’ spaces in the Dataset. Filtering Values and creating Conditional Column. Removing duplicate entries. This was done with PostgreSQL

Data Transformation this is part of the Data cleaning process where I deleted the “Note” Column because it has no usefulness in the insight.

### Exploratory Data Analysis (EDA)

With PostgreSQL

- Total Number of Incidents
- Average Age of Victims
- Number of Male
- Number of Female
- Number of Incidents by Month:
- Percentage of Participants in Hostilities
- Top 5 Locations with the Highest Number of Incidents
- Average Age of Victims by Gender
- Number of Incidents by Citizenship 

### Data Analysis

This is the query process taken to analyze the dataset on PostgreSQL.
~~~
CREATE TABLE War (
    victim_name VARCHAR(255),
    date_of_event DATE,
    year INTEGER,
    month VARCHAR(255),
    age INTEGER,
    citizenship VARCHAR(255),
    event_location VARCHAR(255),
    event_location_district VARCHAR(255),
    event_location_region VARCHAR(255),
    date_of_death DATE,
    gender VARCHAR(50),
    took_part_in_the_hostilities VARCHAR(50),
    place_of_residence VARCHAR(255),
    place_of_residence_district VARCHAR(255),
    type_of_injury VARCHAR(255),
    ammunition VARCHAR(255),
    killed_by VARCHAR(255),
    notes TEXT
);

SELECT * FROM WAR;

-- Update the Gender column with standardized values
UPDATE War
SET gender = CASE
    WHEN LOWER(gender) = 'male' OR LOWER(gender) = 'm' THEN 'male'
    WHEN LOWER(gender) = 'female' OR LOWER(gender) = 'f' THEN 'female'
    ELSE gender
END;

UPDATE War
SET gender = 
    CASE 
        WHEN LOWER(gender) = 'maleale' THEN 'male'
        WHEN LOWER(gender) = 'femaleemale' THEN 'female'
        ELSE gender
    END;
	
-- Check if the table is updated
SELECT DISTINCT gender FROM War;

--Remove/Update 'Null" values'
-- Replace NULL values in the 'gender' column with 'N/A'
UPDATE War
SET gender = COALESCE(gender, 'N/A');

-- Replace NULL values in the 'age' column with 0
UPDATE War
SET age = COALESCE(age, 0);

-- Replace NULL values in the 'took_part_in_the_hostilities' column with '0'
UPDATE War
SET took_part_in_the_hostilities = COALESCE(took_part_in_the_hostilities, '0');

SELECT * FROM WAR;

-- Replace NULL values in the 'Ammunition' column with 'N/A'
UPDATE War
SET Ammunition = COALESCE(Ammunition, 'N/A');

-- Replace NULL values in the 'Notes' column with 'N/A'
UPDATE War
SET Notes = COALESCE(Notes, 'N/A');

-- Total Number of Incidents:
SELECT COUNT(*) AS total_incident FROM war;
--=total_incident =11,124

--Average Age of Victims:
SELECT AVG(age) AS average_age FROM WAR;
--= Average_age =26.7

--Number of Male:
SELECT COUNT(*) AS male_count
FROM War
WHERE LOWER(gender) = 'male';
-- male_count =9681

--Number of Female:
SELECT COUNT(*) AS female_count
FROM War
WHERE LOWER(gender) = 'female';
-- female_count=1423

-- Number of Incidents by Month:
SELECT month, COUNT(*) AS incidents_by_month
FROM War
GROUP BY month, date_of_event
ORDER BY EXTRACT(MONTH FROM date_of_event);

--Percentage of Participants in Hostilities:
SELECT took_part_in_the_hostilities, 
COUNT(*) * 100.0 / (SELECT COUNT(*) 
FROM War) AS percentage 
FROM War GROUP BY took_part_in_the_hostilities;

-- Round up the above to two decimal places
SELECT
  took_part_in_the_hostilities,
  ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM War), 2) AS percentage
FROM War
GROUP BY took_part_in_the_hostilities;

"took_part_in_the_hostilities"	"percentage"
--"No"	                        41.83
--"Object of targeted killing"	1.80
--"Unknown"	                5.42
--"0"	                        12.86
--"Israelis"	                6.93
--"Yes"	                        31.17 

--Top 5 Locations with the Highest Number of Incidents:
SELECT event_location, 
COUNT(*) AS incident_count 
FROM War GROUP BY event_location 
ORDER BY incident_count 
DESC LIMIT 5;

--"event_location"	"incident_count"
--"Gaza City"	         2232
--"Rafah"	         832
--"Khan Yunis"           538
--"Jabalya R.C."	 477
--"Beit Lahiya"	         471

--Average Age of Victims by Gender:
SELECT gender, AVG(age) AS average_age 
FROM War 
GROUP BY gender;
-- Round up the above to two decimal places
SELECT
  gender,
  ROUND(AVG(age), 2) AS average_age
FROM War
GROUP BY gender;

--"gender"	"average_age"
--"female"	29.57
--"N/A"	        0.00
--"male"	26.03

--Number of Incidents by Citizenship:
SELECT citizenship, 
COUNT(*) AS incidents_by_citizenship 
FROM War 
GROUP BY citizenship;

--"citizenship"	"incidents_by_citizenship"
--"American"	  1
--"Palestinian"	  10092
--"Jordanian"	  2
--"Israeli"	  1029

SELECT * FROM WAR;

END
~~~

### Individual Visuals

-**Fatalities by Year**

![1 (2)](https://github.com/Solution92/Analysis-of-Israeli-Palestinian-Conflict-from-2000-to-2023-Using-Power-BI/assets/144762124/445e7d8e-8e54-4fb3-95a7-56d7f85eae40)

- **%GT Sum of Age by Gender**

![2](https://github.com/Solution92/Analysis-of-Israeli-Palestinian-Conflict-from-2000-to-2023-Using-Power-BI/assets/144762124/2f0e0535-9f0e-4cd7-b67e-7ef387309d3b)

- **Fatalities by Event_Location_Region and Event_Location_District**

![3](https://github.com/Solution92/Analysis-of-Israeli-Palestinian-Conflict-from-2000-to-2023-Using-Power-BI/assets/144762124/d33ef9a0-e15a-49f0-bda6-5c8fc51925d4)

- **Fatalities by Type_of_Injury**

![4](https://github.com/Solution92/Analysis-of-Israeli-Palestinian-Conflict-from-2000-to-2023-Using-Power-BI/assets/144762124/4e2b9f43-31be-474c-9a50-b13c8396c885)

- **%GT AgeCounts by Age**

![5](https://github.com/Solution92/Analysis-of-Israeli-Palestinian-Conflict-from-2000-to-2023-Using-Power-BI/assets/144762124/07440387-b6f6-41c2-a2b9-3b7050349e87)

- **Fatalities by Citizenship**

![6](https://github.com/Solution92/Analysis-of-Israeli-Palestinian-Conflict-from-2000-to-2023-Using-Power-BI/assets/144762124/18d292f3-7cf1-4ce7-a58e-b5d7e52acebb)

- **Count of type_of_injury by took_part_in_the_hostilities and type_of_injury**

![7](https://github.com/Solution92/Analysis-of-Israeli-Palestinian-Conflict-from-2000-to-2023-Using-Power-BI/assets/144762124/ec09dd87-332b-46a5-a5c4-fd0c7d3107c1)

-**Count of Killed_by by Ammunition and killed_by**

![8](https://github.com/Solution92/Analysis-of-Israeli-Palestinian-Conflict-from-2000-to-2023-Using-Power-BI/assets/144762124/fa6e77ae-9692-41ff-bb76-9b8c4789ce4b)

## The Dashboard

![Screenshot_138](https://github.com/Solution92/Analysis-of-Israeli-Palestinian-Conflict-from-2000-to-2023-Using-Power-BI/assets/144762124/28364246-b3e6-4701-ab7c-f29a99de3aa8)


### Result and Findings

- Fatalities trended up, resulting in a 614.29% increase between 2000 and 2023.  Fatalities started trending up in 2017, rising by 212.50% (170) in 6 years.  Fatalities jumped from 35 to 681 during its steepest incline between 2000 and 2006.  
- Fatalities were highest for gunfire at 9,849, followed by explosion and shelling.  
- At 10092, Palestinians had the highest Fatalities and were 1,009,100.00% higher than Americans, which had the lowest Fatalities at 1.  Palestinian had the highest Fatalities at 10,092, followed by Israeli, Jordanian, and American.  Across all 4 citizens, Fatalities ranged from 1 to 10,092.  
- No in type_of_injury gunfire made up 40.27% of the Count of type_of_injury.  The average Count of type_of_injury was highest for gunfire at 1,641.50, followed by explosion and shelling.  
- Israeli security forces had the highest total Count of killed_by at 10,000, followed by Palestinian civilians at 1,028 and Israeli civilians at 96.  N/A in killed_by Israeli security forces made up 43.83% of the Count of killed_by.  Israeli security forces had the highest average Count of killed_by at 625, followed by Palestinian civilians at 79.08 and Israeli civilians at 32. 

### Recommendation

- Based on the findings, it is evident that the trend of fatalities has increased significantly over the years, with a notable rise starting in 2017. 
- The disproportionate impact on Palestinian citizens raises concerns and underscores the importance of addressing the root causes of conflict. 
- The involvement of Israeli security forces in a substantial number of fatalities suggests a need for careful examination and potentially reevaluating engagement strategies to minimize civilian casualties. 
- This analysis highlights the urgency of diplomatic efforts and conflict resolution to mitigate the human cost of ongoing hostilities.

### Limitations

- The dataset limitations may include a lack of detailed contextual information surrounding each fatality, such as the specific circumstances, motivations, or geopolitical factors that could provide a more nuanced understanding. 
- Additionally, the dataset may not capture potential biases in reporting or underreporting of incidents, impacting the accuracy of the trends and statistics. 
- The absence of demographic details beyond age, gender, and citizenship limits the depth of analysis on the diverse factors influencing fatalities. 
- The dataset may not differentiate between combatants and non-combatants, making it challenging to assess the impact on civilian populations accurately. 
- Lastly, the dataset's time range and geographic scope may not encompass all relevant events, potentially overlooking patterns or trends outside the specified parameters

### References
[Israeli-Palestinian Conflict](https://www.kaggle.com/datasets/willianoliveiragibin/fatalities-in-the-israeli-palestinian)













