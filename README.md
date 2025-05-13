# Unlocking-Hidden-Insights-To-A-Deep-Dive-into-Marketing-Campaign-Performance-with-SQL
SQL-based analysis of digital marketing campaign data to evaluate ad performance, optimize ROI, and support data-driven marketing strategies.

![](https://github.com/judoski366/Unlocking-Hidden-Insights-To-A-Deep-Dive-into-Marketing-Campaign-Performance-with-SQL/blob/main/Marketing%20Image.jpg)

## Table Of Content
 - [Introduction](#Introduction)
 - [ASK](#ASK)
 - [PREPARE](#PREPARE)
 - [PROCESS](#PROCESS)
 - [ANALYZE](#ANALYZE)
 - [SHARE](#SHARE)
 - [ACT](#ACT)

## INTRODUCTION


In today’s rapidly evolving digital landscape, marketing strategies have become increasingly reliant on data-driven insights to reach and engage potential customers. Our company has been actively running digital marketing campaigns across platforms such as Facebook, Instagram, and Pinterest, with a strong focus on maximizing return on investment (ROI) and promoting brand visibility.

To support these efforts, we have systematically collected comprehensive daily performance data, including key metrics such as impressions, clicks, spend, conversions, and engagement indicators like likes, shares, and comments. This project leverages SQL to perform a detailed analysis of that data, with the goal of evaluating campaign effectiveness, identifying trends, and uncovering opportunities for strategic optimization.

As a data analyst for the company, I led the effort to transform raw campaign data into actionable insights using the six phases of the data analysis process:

1. **Ask** – Defining key business questions around marketing performance and ROI
2. **Prepare** – Gathering and organizing ad performance data from multiple platforms
3. **Process** – Cleaning, structuring, and validating the data for analysis
4. **Analyze** – Writing SQL queries to explore performance metrics and uncover insights
5. **Share** – Presenting findings through visualizations and summary reports
6. **Act** – Recommending data-driven improvements to campaign strategies

This structured approach demonstrates the power of SQL in unlocking the value of marketing data, facilitating informed decision-making, and driving more efficient and impactful advertising campaigns.

---

## ASK

The Problem Statement, this project aims to conduct a comprehensive SQL analysis of our campaign data, we aim to uncover trends, patterns, and correlations that will enable us to optimize ad spend, target audiences more effectively, and measure the return on investment (ROI) of our marketing efforts.

### Objectives

* **Evaluate Campaign Performance:** Assess the overall performance of each campaign in terms of reach, engagement, and conversions.

* **Channel Effectiveness:** Determine which advertising channels are driving the best results.

* **Geographical Insights:** Identify the cities that show the highest engagement and conversion rates.

* **Device Performance:** Understand how ads perform across different devices.

* **Ad-Level Analysis:** Analyze the performance of individual ads to identify high-performing creatives.

* **ROI Calculation:** Calculate the return on investment (ROI) for each campaign.

---

## PREPARE

### Data Description
The CSV file, with total rows of 9,900, has 18 columns which include the following:

* **Campaign:** Name of the marketing campaign.

* **Date:** Date for daily ad performance metrics.

* **City/Location:** Cities that were targeted by the campaign.

* **Latitude:** Latitude for the cities.

* **Longitude:** Longitude for the cities.

* **Channel:** Channel where ads were displayed (Facebook, Instagram, Pinterest).

* **Device:** Device from which ads were viewed.

* **Ad:** Ads used within a campaign.

* **Impressions:** Daily impressions (times ad was shown to a viewer) for each ad.

* **CTR, %:** Daily average click-through rate for each ad.

* **Clicks:** Daily clicks for each ad.

* **Daily Average CPC:** Daily average cost-per-click for each ad.

* **Spend, GBP:** Total daily amount of advertising spending for each ad, in GBP.

* **Conversions:** Total daily purchases attributed to a specific ad.

* **Total conversion value, GBP:** Total amount in GBP received from purchases attributed to a specific ad.

* **Likes:** Total daily likes (or other reactions) per ad.

* **Shares:** Total daily shares per ad. For simplicity, each Pin on Pinterest is counted as a share.

* **Comments:** Total daily comments per ad.

---

## PROCESS

The tool used for this analysis is Microsoft SQL Server Management Studio (SSMS) for querying the database and Excel for Data Visualization.
On exploring the datasets, I observed that the data is clean and suitable for analysis. There were no problems such as duplicates, missing numbers, or wrong data types with the data. The data is consistent and accurate. The tables are free of outliers that could pose vague insight to our study. Therefore, there are no obvious cleaning and transformation operations required upon explorations. Except for the project objective questions on the average CPC and CTR where Data cleaning was needed for removing % from the CTR column, Then I converted the CTR column from NVARCHAR datatype to FLOAT datatype to enable me to carry out efficient analysis.

**Before Cleaning**

![](https://github.com/judoski366/Unlocking-Hidden-Insights-To-A-Deep-Dive-into-Marketing-Campaign-Performance-with-SQL/blob/main/Before%20cleaning.png)

**After Cleaning**

![](https://github.com/judoski366/Unlocking-Hidden-Insights-To-A-Deep-Dive-into-Marketing-Campaign-Performance-with-SQL/blob/main/After%20Cleaning.png)


The first image is before cleaning the data the CTR column contains % as a varchar data type, the second image is after the cleaning as it was changed to a float data type.

---

## ANALYZE & SHARE

### Campaign Performance:

* Which campaign generated the highest number of impressions, clicks, and conversions?

<pre> ```sql SELECT Campaign, 
  ROUND(Sum(impressions), 0) AS Total_Impressions, 
  SUM(clicks) AS Total_Clicks,
  SUM(conversions) AS Total_conversions
FROM Campaign
GROUP BY Campaign
ORDER BY Total_impressions DESC, 
Total_Clicks DESC, 
Total_Conversions DESC; ``` </pre>
