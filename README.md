# Bellabeat
Competitive analysis of Fitbit dataset for Bellabeat case study using SQL.  You can find the tableau visualisation for this case study [here](https://public.tableau.com/app/profile/ashleigh.eaves/viz/BellabeatDraft/Dashboard)

# Scenario
Bellabeat, a high-tech manufacturer of health-focused products, are a small player looking to expand their business in the global smart device market. Bellabeat’s product line is made up of the Bellabeat app, which allows users insight into their health by providing data on their activity, sleep, stress, menstrual cycle, and mindfulness habits. The Bellabeat app also connects to the company’s line of wearable smart device products. For the purpose of this case study, I will be focusing on the Leaf product, which can be worn as a bracelet, necklace or clip and tracks user activity, sleep and stress. 

Cofounder Urska Srsen believes analyzing smart device fitness data could help unlock new growth opportunities for the company. Urska has asked me to focus on one of Bellabeat’s products and analyze smart device data to gain insight into how customers are using their smart devices. The insights uncovered will then help guide marketing strategy for the company. Urska points me to some FitBit Fitness Tracker data, a publicly available dataset from 30 fitbit users. The data includes minute level output for physical activity, heart rate, weight, steps and sleep tracking. 

# Data preparation, cleaning and formatting
Limitations for this data exist due to the sample size and absence of key characteristics of the participants, such as gender, age, location, lifestyle. For this analysis the datasets for daily activity, daily calories, daily intensities, daily steps, daily sleep, and weight log information, will be used. I will be analysing the data using SQL via Bigquery and visualizing key findings using Tableau Public.  

First things first, I check how many unique users there are in each table. There were 33 users who tracked Daily Activity, Daily Steps, Daily Calories, Hourly Steps and Hourly Calories. 24 Users logged their sleep and only 8 users updated their weight in the time studied. I find that the sleep table has 3 duplicate entries. I find and remove these entries from the table using excel and reupload the dataset. The daily and hourly activity, steps and calories data sets have the highest numbers of average days logged at just over 28 each. The sleep dataset only has an average of 17 days logged, and the weight dataset has an average of just 8 days logged. 

I reformat the dates for each dataset as much as possible by pulling out the month date and day of week from each table. I figure I might want to get an idea of how often users are actually wearing their devices, so I add a column for total minutes worn. I want to get a general sense of the weekly and monthly trends for activity, so I pull out the average numbers of steps and calories burnt by day of month and day of week. 

# Overall Trends 

# Activity 
I wanted to understand how active the FitBit sample users were. So I decide to categorise them by activity level, using their average daily steps. The Australian Beauro of Statistics collects data on Australians levels of physical activity. According to the latest Australian Health Survey, the average Australian adult takes 7,400 steps per day. Using this information, I create 4 categories of activity from the FitBit users. Users who take less then 4,000 steps are classified as “Inactive”, users who take between 4001 and 7000 steps on average are classified as “Light”, users who take between 7001 and 10000 steps are classified as “Average” and users who take more than 10,000 steps on average are classified as “Very” active. From this, I can see that user activity was well distributed amongst the small FitBit sample, with 18% of users considered “inactive”, 21% of users considered “lightly” active, another 21% of users are considered “Very” Active and, as expected, a larger number of users (39%) considered “Averagely” active. 

# Sleep 
The Australian institute of health and welfare recommends that the average adult needs a minimum of 7 hours of sleep per night. I run a query to find out the total number of hours of sleep each user received over the month, and created an estimate for the average number this would be per night. The results of this query are concerning. Of all 24 users who logged their sleep, only 4 of them met the recommended guidelines, and 9 users logged an average of 2 hours or fewer per night. This leads me to believe that either the sleep tracker data is not actually tracking users sleep correctly or there is not enough data to base averages on. I remember that this dataset only had an average of 17 days logged per user. I consider removing null values from the analysis, however, when inspecting the dataset more closely, I see that there are many records with questionable data. For example there are 15 entries which log the minute asleep time as less than 2 hours, and 4 in which the user slept for more than 12. While these numbers are technically possible, they are unlikely, and combined with the small data set, I decide that the sleep data is not reliable enough to generate an accurate reflection of the type of sleep the FitBit sample are getting. 

# Time of Day
To understand how users activity levels changed throughout the day, I create an average calorie and average step metric and break this down by hour. FitBit users activity levels generally peak in two day parts, firstly around lunch time and secondly in the early evening. Unsurprisingly, the data also indicates that calorie burn and step count are closely related.  

To understand how users engaged with physical activity from a weekly perspective, I also create a query showing day of week with average active minutes per day, and create a "moderately active minutes" column (by adding “very active” an “fairly active” minutes together, and join the two tables on Day of week.While we see the average count for “moderate” activity peaking on Mondays and Tuesdays, the average count of any active minutes peaks on Fridays and Saturdays. Additionally, we see the average number of moderately active minutes exceed the average for any active minutes earlier in the week. This would suggest that dedicated exercise was occurring more on Sunday, Monday and Tuesday, while incidental exercise was driving activity levels on Friday and Saturday.   

# Device Usage by UserType 
Since we have a high number of users logging their activity everyday, I looked into how device usage differs by usership type. To investigate this, I first assign each user a “usership type” based on the percentage of the entire month in which their device was worn. There are 13 users who wore the device for more than 90% of the month, 14 users who wore the device for between 65 and 90% of the month and 6 users who wore the device for less than 65% of the month. 

Looking at the average sign ins, there does not seem to be much difference between high and medium user types with the high users logging in on average 31 days and the medium users logging 30 days. Low users logged an average of 20 days. Clearly, it the amount of time they are wearing the device throughout the day that’s driving the total difference in worn minutes in usertype, rather than the number of days they use the device per month. Poor battery life or users removing the device for other reasons are potential causes of this data loss. 

# Sleep & Weight Log Ins By Usertype
I want to look at which usertypes are also tracking their sleep and weight, so I use a join function to add usertype to these datasets. Medium frequency users were most likely to track their weight, with 52% of weight log entries from this user base. There were no weight log entries from the low usertype.  Medium frequency users were also the most likely to track their sleep, with 75% off all sleep entries coming from this usertype. 

# Comparison to the average Australian
The Australian Department of Health’s physical activity guidelines recommend that people aged 18-64 years should be active on most days of the week (at least 5) and complete at least 150 of moderate to vigorous intensity exercise per week. How do our FitBit users compare with this? 

If we define “activity” as any level of moderate or vigorous activity, only 13 of our 33 users met the 5 day per week goal (when using an average based on their monthly data). However, 71% of the Fitbit sample do meet the minimum 150 minutes per week benchmark. Taken together, 12 of our 33 (36%) of our users are reaching these combined physical activity guidelines, compared to 28% of the average Australian. 

# Marketing Recommendations 
1.	Given the consistency with which the FitBit dataset wore their devices, Bellabeat should market the ways in which the Leaf is better able to track minute by minute data than competitors for consistent, more informed health insights. 
2.	Similarly, Bellabeat should promote the Leaf’s ability to reliably track user sleep, since FitBit users struggled to do so. 
3.	While FitBit users did not exercise more than the average Australian, they were more likely to meet the Australian Department of Health’s physical activity guidelines than the average Australia. Bellabeat could promote their Leaf device with messaging highlighting the department of health’s recommendations and positioning Leaf as a tool to help reach women’s health goals. 




