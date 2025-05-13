# Unlocking-Hidden-Insights-To-A-Deep-Dive-into-Marketing-Campaign-Performance-with-SQL
SQL-based analysis of digital marketing campaign data to evaluate ad performance, optimize ROI, and support data-driven marketing strategies.

![](https://github.com/judoski366/Unlocking-Hidden-Insights-To-A-Deep-Dive-into-Marketing-Campaign-Performance-with-SQL/blob/main/Marketing%20Image.jpg)

## Table Of Content
 - [Introduction](#Introduction)
 - [ASK](#ASK)
 - [PREPARE](#PREPARE)
 - [PROCESS](#PROCESS)
 - [ANALYZE-SHARE](#ANALYZE-SHARE)
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

## ANALYZE SHARE

### Campaign Performance:

- **Which campaign generated the highest number of impressions, clicks, and conversions?**

```sql
-- Which campaign generated the highest number of impressions, clicks, and conversions
SELECT Campaign,
  ROUND(SUM(impressions), 0) AS Total_Impressions,
  SUM(clicks) AS Total_Clicks,
  SUM(conversions) AS Total_Conversions
FROM Campaign
GROUP BY Campaign
ORDER BY Total_Impressions DESC,
  Total_Clicks DESC,
  Total_Conversions DESC;
```

![](https://github.com/judoski366/Unlocking-Hidden-Insights-To-A-Deep-Dive-into-Marketing-Campaign-Performance-with-SQL/blob/main/Campaign%20Performance.png)


**Analysis** 

* Based on the data analysis, it has been observed that the fall season has the highest number of campaigns, followed by the spring season. Conversely, the summer season has the least number of campaigns. This information is based on the total impressions, clicks, and conversions recorded in the data.


**What is the average cost-per-click (CPC) and click-through rate (CTR) for each campaign?**

 ```sql
--- What is the average cost-per-click (CPC) and click-through rate (CTR) for each campaign?

SELECT Campaign,
 ROUND(AVG(CAST(daily_average_cpc AS FLOAT)), 2) AS Avg_cpc,
 ROUND(AVG(CAST(CTR AS FLOAT)), 2) AS Avg_CTR
FROM Campaign
GROUP BY Campaign
ORDER BY Avg_Cpc DESC;
```

![](https://github.com/judoski366/Unlocking-Hidden-Insights-To-A-Deep-Dive-into-Marketing-Campaign-Performance-with-SQL/blob/main/Average%20CPC.png)

**Analysis:** 

* The average cost per click (CPC) for the fall campaign is 1.35%, for the spring campaign it is 1.23%, and for the summer campaign it is 1.13%. The average click-through rate (CTR) for the fall campaign is 0.93%, for the spring campaign it is 0.86%, and for the summer campaign, it is 0.92%.

* The fall campaign stands out with the highest average CPC and average CTR, indicating a peak in performance compared to the spring and summer campaigns. There are noticeable variations in both average CPC and average CTR between the summer and spring campaigns.


### Channel Effectiveness:

**Which channel has the highest ROI?**

 ```sql
--- Which channel has the highest ROI?

SELECT Channel,
  Round(SUM(Total_Conversion_Value_GBP - Spend_GBP) / SUM(Spend_GBP), 2) AS ROI
  FROM Campaign
  GROUP BY Channel
  ORDER BY ROI DESC;
 ```

![](https://github.com/judoski366/Unlocking-Hidden-Insights-To-A-Deep-Dive-into-Marketing-Campaign-Performance-with-SQL/blob/main/Highest%20ROI%20Image.png)

**Analysis:**

* According to the analysis, Pinterest has been found to have the highest Return on Investment (ROI) at 21.51%. Following Pinterest, Instagram comes in second with a considerably lower ROI of 9.80%, and Facebook trails behind with the lowest ROI at 4.76%

**How do impressions, clicks, and conversions vary across different channels?**

 ```sql
--- How do impressions, clicks, and conversions vary across different channels?

SELECT Channel, 
  ROUND(Sum(impressions), 0) AS Total_Impressions, 
  SUM(clicks) AS Total_Clicks,
  SUM(conversions) AS Total_conversions
FROM Campaign
GROUP BY Channel
ORDER BY Total_impressions DESC, 
Total_Clicks DESC, 
Total_Conversions DESC;
```

![](https://github.com/judoski366/Unlocking-Hidden-Insights-To-A-Deep-Dive-into-Marketing-Campaign-Performance-with-SQL/blob/main/Impression%20Image.png)

**Analysis**

In my analysis across all the marketing channels, I found that Facebook consistently generates the highest number of clicks and impressions. However, the most interesting finding is that Instagram delivers the highest conversion rate, with an impressive 15.95k conversions. This suggests that while Facebook may drive more traffic, Instagram is more effective at turning that traffic into actual conversions.

### Geographical Insights:

**Which cities have the highest engagement rate (likes, shares, comments)?**

```sql
---Which cities have the highest engagement rates (likes, shares, comments)?

SELECT city_location,
  SUM(Likes_Reactions + Shares + Comments) / COUNT(*) AS Engagement_Rate
FROM Campaign
GROUP BY City_Location
ORDER BY Engagement_Rate DESC;
```

![](https://github.com/judoski366/Unlocking-Hidden-Insights-To-A-Deep-Dive-into-Marketing-Campaign-Performance-with-SQL/blob/main/Engagment%20By%20city.png)

**Analysis** 

Based on the chart, it is evident that the city of London has the highest engagement rate, accounting for 86% of the ad interactions, surpassing all other locations. Following closely behind is the city of Manchester.

**What is the conversion rate by city?**

```sql
--- What is the conversion rate by city?

SELECT City_location,
  SUM(Conversions) As Conversion_Rate
FROM Campaign
GROUP BY City_Location
ORDER By Conversion_Rate DESC;
``` 

![](https://github.com/judoski366/Unlocking-Hidden-Insights-To-A-Deep-Dive-into-Marketing-Campaign-Performance-with-SQL/blob/main/Conversion%20By%20City.png)

**Analysis**

According to the data presented in the chart, it is clear that the city of Manchester has the highest conversion rate, with a total number of 14.4k, surpassing all other cities. Following closely behind is the city of London. This indicates that Manchester has a very strong performance in terms of conversions compared to other cities.

### Device Performance

**How do ad performances compare across different devices (mobile and desktop)?**

```sql
--- How do ad performances compare across different devices (mobile and desktop)?

SELECT Device,
  ROUND(Sum(impressions), 0) AS Total_Impressions, 
  SUM(clicks) AS Total_Clicks,
  SUM(conversions) AS Total_conversions
FROM Campaign
GROUP BY Device
ORDER BY Total_impressions DESC, 
  Total_Clicks DESC, 
  Total_Conversions DESC;
```

![](https://github.com/judoski366/Unlocking-Hidden-Insights-To-A-Deep-Dive-into-Marketing-Campaign-Performance-with-SQL/blob/main/Device%20Performance.png)

**Analysis** 

When comparing ad performance across different devices, I find that the total clicks and impressions are high on mobile. Surprisingly, the desktop has the highest conversion rate at 21.31k.

**Which specific ads are performing best in terms of engagement and conversions?**

```sql
--- Which specific ads are performing best in terms of engagement and conversions?

SELECT Ad, 
  SUM(conversions) AS Total_conversions,
  SUM(Likes_Reactions + Shares + Comments) AS Total_Engagements
FROM Campaign
GROUP BY Ad
ORDER BY Total_Conversions DESC,
  Total_Engagements DESC;
```

![](https://github.com/judoski366/Unlocking-Hidden-Insights-To-A-Deep-Dive-into-Marketing-Campaign-Performance-with-SQL/blob/main/Ad%20performance.png)

**Analysis** 

Based on the data presented in the chart, it is clear that the Discount Ad outperforms the collection Ad in terms of both conversions and engagement. The Discount Ad shows a total of 21.18k conversions and 460M engagements, indicating its superior performance compared to the collection Ad.


### Return On Investment (ROI) Calculation:

We understand that ROI is the profit made / cost of investment.

**What is the ROI for each campaign, and how does it compare across different channels and devices?**

```sql
---What is the ROI for each campaign, and how does it compare across different channels and devices?

SELECT Campaign,
  
  ROUND(SUM(Total_conversion_value_GBP - Spend_GBP) / SUM(SPEND_GBP), 2) AS ROI
FROM Campaign
GROUP BY Campaign 
ORDER BY ROI DESC;

SELECT Channel,
  SUM(Total_conversion_value_GBP) / SUM(SPEND_GBP) AS ROI
FROM Campaign
GROUP BY Channel
ORDER BY ROI DESC;


SELECT Device,
  SUM(Total_conversion_value_GBP) / SUM(SPEND_GBP) AS ROI
FROM Campaign
GROUP BY Device
ORDER BY ROI DESC;
```

![](https://github.com/judoski366/Unlocking-Hidden-Insights-To-A-Deep-Dive-into-Marketing-Campaign-Performance-with-SQL/blob/main/All%20ROI.png)

**Analysis**

Based on the analysis of the chart, it can be observed that the return on investment (ROI) is particularly high during the summer campaign, reaching 13.1%. Among the various channels, Pinterest stands out with an impressive ROI of 22.5%, while desktop devices also perform well, yielding an ROI of 11.0%. Conversely, Facebook shows the least ROI at 5.8%, particularly during the Fall Campaign with an ROI of 8.5%. On mobile devices, the ROI is relatively lower at 10.2%.

**How does ‘spend’ correlate with ‘conversion’ value across different campaigns?**

```sql
---How does spend correlate with conversion value across different campaign
SELECT Campaign,
  SUM(Spend_GBP) AS Total_Spend,
  ROUND(SUM(Total_Conversion_Value_GBP), 1) AS Total_Conversion
FROM Campaign
GROUP BY Campaign
ORDER BY Total_Spend DESC,
Total_Conversion DESC;
```

![](https://github.com/judoski366/Unlocking-Hidden-Insights-To-A-Deep-Dive-into-Marketing-Campaign-Performance-with-SQL/blob/main/Correlation%20by%20campaign.png)

**Analysis** 
Upon analysis, it is evident that there is a strong positive correlation between the amount spent and the ‘conversion’ rate across all campaigns. Both of these variables exhibit their highest values during the Fall campaign and their lowest values during the Summer campaign. This suggests that increased spending during the Fall campaign leads to higher conversion rates, whereas reduced spending during the Summer campaign results in lower conversion rates.

### ACT

#### my recommendation for the analysis done on the marketing campaign

* The fall season has the highest campaign metrics, making it the most effective season for campaigns. Consider allocating more budget towards fall campaigns to maximize impressions and conversions. Review and refine ad content and targeting strategies for spring and summer campaigns to enhance performance.

* Consider prioritizing Pinterest for future campaigns due to its high ROI of 21.51%. While Instagram has strong conversion rates, its ROI is lower compared to Pinterest. Improve Instagram’s ROI by refining targeting and costs. Focus on improving Facebook’s current ROI of 4.76% through more precise ad spending and leveraging high-performing ad formats.

* For high ad performance, discount ads demonstrate the highest engagement at 460 million and the most conversions at 21.18 thousand. It is recommended to prioritize discount ad strategies to maintain high performance. Additionally, collection ads underperform in comparison to discount ads. Therefore, it is suggested to evaluate and adjust the content, targeting, and delivery of collection ads to enhance their effectiveness.

**To Summarize, Allocate more funds to high ROI channels and devices. Increase investment in Pinterest and desktop devices. Adjust budget allocations to ensure funds are directed towards the most effective channels. Implement A/B testing for ad formats and targeting strategies to optimize campaigns. Increase the budget for fall campaigns due to superior performance. Allocate resources to enhance summer campaigns.**

---

**Thank you** for taking the time to read my analysis! I truly appreciate your interest and support. If you’d like to stay connected and explore more insights or collaborate, feel free to connect with me on my [LinkedIn](https://www.linkedin.com/in/nwagu-jude/), [Twitter/X](https://x.com/@jcndata). Your engagement means a lot, and I look forward to sharing more valuable content with you
