# Workforce_Management

# YouTube Channel Engagement Analysis


![excel-to-powerbi-animated-diagram](asset/images/kaggle_to_powerbi.gif)




# Table of contents 

- [Objective](#objective)
- [Data Source](#data-source)
- [Stages](#stages)
- [Design](#design)
  - [Mockup](#mockup)
  - [Tools](#tools)
- [Development](#development)
  - [Pseudocode](#pseudocode)
  - [Data Exploration](#data-exploration)
  - [Data Cleaning](#data-cleaning)
  - [Transform the Data](#transform-the-data)
  - [Create the SQL View](#create-the-sql-view)
- [Testing](#testing)
  - [Data Quality Tests](#data-quality-tests)
- [Visualization](#visualization)
  - [Results](#results)
  - [DAX Measures](#dax-measures)
- [Analysis](#analysis)
  - [Findings](#findings)
  - [Validation](#validation)
  - [Discovery](#discovery)
- [Recommendations](#recommendations)
  - [Potential ROI](#potential-roi)
  - [Potential Courses of Actions](#potential-courses-of-actions)
- [Conclusion](#conclusion)




# Objective 

- What is the key pain point? 

JJ Olatunji has been assigned to promote the England national team's jersey for the World Cup. He wants to determine which of his three YouTube channels—KSI, Sidemen, or JJ Olatunji—would be the most effective for promoting the England jersey for the 2025 World Cup. He needs data-driven insights to decide which channel will generate the highest engagement and visibility for the campaign.


- What is the ideal solution? 

To create a dashboard that provides insights into the performance of JJ Olatunji’s three channels, including:
- subscriber count
- total views
- total videos, and
- engagement metrics

This dashboard will help JJ and his team decide which channel to use for the England jersey promotion and future marketing campaigns.

## User story 

As JJ Olatunji, I want to use a dashboard to analyze my three YouTube channels (KSI, Sidemen, and JJ Olatunji) and determine which has the highest engagement and reach.

This dashboard should allow me to compare performance metrics such as subscriber growth, average views, and audience interaction to ensure that I choose the most effective channel for promoting the England jersey for the 2025 World Cup.

With this information, I can maximize campaign effectiveness and ensure the highest possible impact and engagement from my audience.


# Data source 

- What data is needed to achieve our objective?

We need data on the top UK YouTubers in 2024, including their 
- channel names
- total subscribers
- total views
- total videos uploaded



- Where is the data coming from? 
The data is sourced from Kaggle (an Excel extract), [see here to find it.](https://www.kaggle.com/datasets/bhavyadhingra00020/top-100-social-media-influencers-2024-countrywise?resource=download)


# Stages

- Design
- Development
- Testing
- Analysis 
 


# Design 

## Dashboard components required 
- What should the dashboard contain based on the requirements provided?

To understand what it should contain, we need to figure out what questions we need the dashboard to answer:

1. Who are the top 10 YouTubers with the most subscribers?
2. Which of JJ's 3 channels has uploaded the most videos?
3. Which of JJ's 3 channels has the most views?
4. Which of JJ's 3 channels has the highest average views per video?
5. Which of JJ's 3 channels has the highest views per subscriber ratio?
6. Which of JJ's 3 channels has the highest subscriber engagement rate per video uploaded?

For now, these are some of the questions we need to answer, this may change as we progress down our analysis.; however through 


## Dashboard mockup

- What should it look like? 

Some of the data visuals that may be appropriate in answering our questions include:

1. Table
2. Treemap
3. Scorecards
4. Horizontal bar chart 




![Dashboard-Mockup](asset/images/dashboard_mockup.png)


## Tools 


| Tool | Purpose |
| --- | --- |
| Excel | Exploring the data |
| SQL Server | Cleaning, testing, and analyzing the data |
| Power BI | Visualizing the data via interactive dashboards |
| GitHub | Hosting the project documentation and version control |
| Mokkup AI | Designing the wireframe/mockup of the dashboard | 


# Development

## Pseudocode

- What's the general approach in creating this solution from start to finish?

1. Get the data
2. Explore the data in Excel
3. Load the data into the SQL Server
4. Clean the data with SQL
5. Test the data with SQL
6. Visualize the data in Power BI
7. Generate the findings based on the insights
8. Write the documentation + commentary
9. Publish the data to GitHub Pages

## Data exploration notes

This is the stage where you have a scan of what's in the data, errors, inconsistencies, bugs, weird and corrupted characters, etc  


- What are your initial observations with this dataset? What's caught your attention so far? 

1. There are at least 4 columns that contain the data we need for this analysis, which signals we have everything we need from the file without needing to contact the client for any more data. 
2. The first column contains the channel ID and what appears to be channel IDS, separated by an @ symbol—we need to extract the channel names from this.
3. Some cells and header names are in a different language. We need to confirm whether these columns are required and, if so, address them.
4. We have more data than we need, so some of these columns would need to be removed





## Data cleaning 
- What do we expect the clean data to look like? (What should it contain? What constraints should we apply to it?)

We aim to refine our dataset to ensure it is structured and ready for analysis. 

The cleaned data should meet the following criteria and constraints:

- Only relevant columns should be retained.
- All data types should be appropriate for the contents of each column.
- No column should contain null values, indicating complete data for all records.

Below is a table outlining the constraints on our cleaned dataset:

| Property | Description |
| --- | --- |
| Number of Rows | 100 |
| Number of Columns | 4 |

Here is a tabular representation of the expected schema for the clean data:

| Column Name | Data Type | Nullable |
| --- | --- | --- |
| channel_name | VARCHAR | NO |
| total_subscribers | INTEGER | NO |
| total_views | INTEGER | NO |
| total_videos | INTEGER | NO |



- What steps are needed to clean and shape the data into the desired format?

1. Remove unnecessary columns by only selecting the ones you need
2. Extract YouTube channel names from the first column
3. Rename columns using aliases







### Transform the data 



```SQL
/*
# 1. Select the required columns
# 2. Extract the channel name from the 'NOMBRE' column
*/

-- 1.
SELECT
    SUBSTRING(NOMBRE, 1, CHARINDEX('@', NOMBRE) -1) AS channel_name,  -- 2.
    total_subscribers,
    total_views,
    total_videos

FROM
    top_uk_youtubers_2024
```


### Create the SQL view 

```SQL
/*
# 1. Create a view to store the transformed data
# 2. Cast the extracted channel name as VARCHAR(100)
# 3. Select the required columns from the top_uk_youtubers_2024 SQL table 
*/

-- 1.
CREATE VIEW view_uk_youtubers_2024 AS

-- 2.
SELECT
    CAST(SUBSTRING(NOMBRE, 1, CHARINDEX('@', NOMBRE) -1) AS VARCHAR(100)) AS channel_name, -- 2. 
    total_subscribers,
    total_views,
    total_videos

-- 3.
FROM
    top_uk_youtubers_2024

```


# Testing 

- What data quality and validation checks are you going to create?

Here are the data quality tests conducted:

## Row count check
```SQL
/*
# Count the total number of records (or rows) in the SQL view
*/

SELECT
    COUNT(*) AS no_of_rows
FROM
    view_uk_youtubers_2024;

```

![Row count check](asset/images/1_row_count_check.png)



## Column count check
### SQL query 
```SQL
/*
# Count the total number of columns (or fields) in the SQL view
*/


SELECT
    COUNT(*) AS column_count
FROM
    INFORMATION_SCHEMA.COLUMNS
WHERE
    TABLE_NAME = 'view_uk_youtubers_2024'
```
### Output 
![Column count check](asset/images/2_column_count_check.png)



## Data type check
### SQL query 
```SQL
/*
# Check the data types of each column from the view by checking the INFORMATION SCHEMA view
*/

-- 1.
SELECT
    COLUMN_NAME,
    DATA_TYPE
FROM
    INFORMATION_SCHEMA.COLUMNS
WHERE
    TABLE_NAME = 'view_uk_youtubers_2024';
```
### Output
![Data type check](asset/images/3_data_type_check.png)


## Duplicate count check
### SQL query 
```SQL
/*
# 1. Check for duplicate rows in the view
# 2. Group by the channel name
# 3. Filter for groups with more than one row
*/

-- 1.
SELECT
    channel_name,
    COUNT(*) AS duplicate_count
FROM
    view_uk_youtubers_2024

-- 2.
GROUP BY
    channel_name

-- 3.
HAVING
    COUNT(*) > 1;
```
### Output
![Duplicate count check](asset/images/4_duplicate_records_check.png)

# Visualization 


## Results

- What does the dashboard look like?

![GIF of Power BI Dashboard](asset/images/top_uk_youtubers_2024.gif)

This shows the Top UK YouTubers in 2024 so far. 


## DAX Measures

### 1. Total Subscribers (M)
```SQL
Total Subscribers (M) = 
VAR million = 1000000
VAR sumOfSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR totalSubscribers = DIVIDE(sumOfSubscribers,million)

RETURN totalSubscribers

```

### 2. Total Views (B)
```SQL
Total Views (B) = 
VAR billion = 1000000000
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR totalViews = ROUND(sumOfTotalViews / billion, 2)

RETURN totalViews

```

### 3. Total Videos
```SQL
Total Videos = 
VAR totalVideos = SUM(view_uk_youtubers_2024[total_videos])

RETURN totalVideos

```

### 4. Average Views Per Video (M)
```SQL
Average Views per Video (M) = 
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR sumOfTotalVideos = SUM(view_uk_youtubers_2024[total_videos])
VAR  avgViewsPerVideo = DIVIDE(sumOfTotalViews,sumOfTotalVideos, BLANK())
VAR finalAvgViewsPerVideo = DIVIDE(avgViewsPerVideo, 1000000, BLANK())

RETURN finalAvgViewsPerVideo 

```


### 5. Subscriber Engagement Rate
```SQL
Subscriber Engagement Rate = 
VAR sumOfTotalSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR sumOfTotalVideos = SUM(view_uk_youtubers_2024[total_videos])
VAR subscriberEngRate = DIVIDE(sumOfTotalSubscribers, sumOfTotalVideos, BLANK())

RETURN subscriberEngRate 

```


### 6. Views per subscriber
```SQL
Views Per Subscriber = 
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR sumOfTotalSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR viewsPerSubscriber = DIVIDE(sumOfTotalViews, sumOfTotalSubscribers, BLANK())

RETURN viewsPerSubscriber 

```




# Analysis 

## Findings

- What did we find?

For this analysis, we're going to focus on the questions below to get the information we need for our marketing client - 

Here are the key questions we need to answer for our marketing client: 
1. Who are the top 10 YouTubers with the most subscribers?
2. Which of JJ's 3 channels has uploaded the most videos?
3. Which of JJ's 3 channels has the most views?
4. Which of JJ's 3 channels has the highest average views per video?
5. Which of JJ's 3 channels has the highest views per subscriber ratio?
6. Which of JJ's 3 channels has the highest subscriber engagement rate per video uploaded?


### 1. Who are the top 10 YouTubers with the most subscribers?

| Rank | Channel Name         | Subscribers (M) |
|------|----------------------|-----------------|
| 1    | NoCopyrightSounds    | 33.60           |
| 2    | DanTDM               | 28.60           |
| 3    | Dan Rhodes           | 26.50           |
| 4    | Miss Katy            | 24.50           |
| 5    | Mister Max           | 24.40           |
| 6    | KSI                  | 24.10           |
| 7    | Jelly                | 23.50           |
| 8    | Dua Lipa             | 23.30           |
| 9    | Sidemen              | 21.00           |
| 10   | Ali-A                | 18.90           |


### 2. Which of JJ's 3 channels has uploaded the most videos?

| Rank | Channel Name    | Videos Uploaded |
|------|-----------------|-----------------|
| 1    | JJ Olatunji     | 1,339           |
| 2    | KSI             | 1,252           |
| 3    | Sidemen         | 349             |



### 3. Which of JJ's 3 channels has the most views?


| Rank | Channel Name | Total Views (B) |
|------|--------------|-----------------|
| 1    | Sidemen      | 6.05            |
| 2    | KSI          | 6.02            |
| 3    | JJ Olatunji  | 4.24            |


### 4. Which of JJ's 3 channels has the highest average views per video?

| Channel Name | Average Views per Video (M) |
|--------------|-----------------|
| Sidemen      | 17.34           |
| KSI          | 4.80            |
| JJ Olatunji  | 3.16            |


### 5. Which of JJ's 3 channels has the highest views per subscriber ratio?

| Rank | Channel Name       | Views per Subscriber        |
|------|-----------------   |---------------------------- |
| 1    | Sidemen            | 288.15                      |
| 2    | JJ Olatunji        | 259.82                      |
| 3    | KSI                | 249.59                      |



### 6. Which of JJ's 3 channels has the highest subscriber engagement rate per video uploaded?

| Rank | Channel Name    | Subscriber Engagement Rate  |
|------|-----------------|---------------------------- |
| 1    | Sidemen         | 60,170                      |
| 2    | KSI             | 19,250                      |
| 3    | JJ Olatunji     | 12,170                      |




### Notes

For this analysis, we'll prioritize analyzing the metrics that are important in generating the expected ROI for our marketing client, which of JJ's YouTube channels with the most 

- subscribers
- total views
- videos uploaded



## Validation 

### 1. Ksi YouTube channel's number of subscribers 

#### Calculation breakdown

Campaign idea = product placement 

a. KSI
- Average views per video = 4.80 million
- Product cost = $5
- Potential units sold per video = 4.80 million x 2% conversion rate = 96,000 units sold
- Potential revenue per video = 96,000  x $5 = $480,000
- Campaign cost (one-time fee) = $50,000
- **Net profit = $480,000 - $50,000 = $430,000**

b. Sidemen

- Average views per video = 17.34 million
- Product cost = $5
- Potential units sold per video = 17.34 million x 2% conversion rate = 346,000 units sold
- Potential revenue per video = 346,000 x $5 = $1,734,000
- Campaign cost (one-time fee) = $50,000
- **Net profit = $1,734,000 - $50,000 = $484,000**

c. JJ Olatunji

- Average views per video = 3.16 million
- Product cost = $5
- Potential units sold per video = 3.16 million x 2% conversion rate = 63,200 units sold
- Potential revenue per video = 63,200 x $5 = $316,000
- Campaign cost (one-time fee) = $50,000
- **Net profit = $316,000- $50,000 = $266,000**


The best option from the category: Sidemen


#### SQL query 

```SQL
/* 

# 1. Define variables 
# 2. Create a CTE that rounds the average views per video 
# 3. Select the column you need and create calculated columns from existing ones 
# 4. Filter results by Youtube channels
# 5. Sort results by net profits (from highest to lowest)

*/


-- 1. 
DECLARE @conversionRate FLOAT = 0.02;		-- The conversion rate @ 2%
DECLARE @productCost FLOAT = 5.0;			-- The product cost @ $5
DECLARE @campaignCost FLOAT = 50000.0;		-- The campaign cost @ $50,000	


-- 2.  
WITH ChannelData AS (
    SELECT 
        channel_name,
        total_views,
        total_videos,
        ROUND((CAST(total_views AS FLOAT) / total_videos), -4) AS rounded_avg_views_per_video
    FROM 
        youtube_db.dbo.view_uk_youtubers_2024
)

-- 3. 
SELECT 
    channel_name,
    rounded_avg_views_per_video,
    (rounded_avg_views_per_video * @conversionRate) AS potential_units_sold_per_video,
    (rounded_avg_views_per_video * @conversionRate * @productCost) AS potential_revenue_per_video,
    ((rounded_avg_views_per_video * @conversionRate * @productCost) - @campaignCost) AS net_profit
FROM 
    ChannelData


-- 4. 
WHERE 
    channel_name in ('KSI', 'Sidemen', 'JJ Olatunji')    


-- 5.  
ORDER BY
	net_profit DESC

```

#### Output

![Most subsc](asset/images/youtubers_with_the_most_subs.png)

### 2. JJ's YouTube channel's number of videos uploaded

### Calculation breakdown 

Campaign idea = sponsored video series  

a. **JJ Olatunji**
- Average views per video = 3,160,000
- Product cost = $5
- Potential units sold per video = 3,160,000 x 2% conversion rate = 63,200 units sold
- Potential revenue per video = 63,200 x $5= $316,000
- Campaign cost (11-videos @ $5,000 each) = $55,000
- **Net profit = $316,000 - $55,000 = $261,00 (profit)**

b. **KSI**

- Average views per video = 4,800,000
- Product cost = $5
- Potential units sold per video = 4,800,000 million x 2% conversion rate = 96,000 units sold
- Potential revenue per video = 96,000  x $5 = $480,000
- Campaign cost (11-videos @ $5,000 each) = $55,000
- **Net profit = $480,000 - $55,000 = $425,00 (profit)**

c. **Sidemen**

- Average views per video = 17,340,000 
- Product cost = $5
- Potential units sold per video = 17,340,000  x 2% conversion rate = 346,000 units sold
- Potential revenue per video = 346,000 x $5 = $1,734,000
- Campaign cost (11-videos @ $5,000 each) = $55,000
- **Net profit = $1,734,000 - $55,000 = $1,679,000 (profit)**


Best option from category: Sidemen

#### SQL query 
```SQL
/* 
# 1. Define variables
# 2. Create a CTE that rounds the average views per video
# 3. Select the columns you need and create calculated columns from existing ones
# 4. Filter results by YouTube channels
# 5. Sort results by net profits (from highest to lowest)
*/


-- 1.
DECLARE @conversionRate FLOAT = 0.02;           -- The conversion rate @ 2%
DECLARE @productCost FLOAT = 5.0;               -- The product cost @ $5
DECLARE @campaignCostPerVideo FLOAT = 5000.0;   -- The campaign cost per video @ $5,000
DECLARE @numberOfVideos INT = 11;               -- The number of videos (11)


-- 2.
WITH ChannelData AS (
    SELECT
        channel_name,
        total_views,
        total_videos,
        ROUND((CAST(total_views AS FLOAT) / total_videos), -4) AS rounded_avg_views_per_video
    FROM
        youtube_db.dbo.view_uk_youtubers_2024
)


-- 3.
SELECT
    channel_name,
    rounded_avg_views_per_video,
    (rounded_avg_views_per_video * @conversionRate) AS potential_units_sold_per_video,
    (rounded_avg_views_per_video * @conversionRate * @productCost) AS potential_revenue_per_video,
    ((rounded_avg_views_per_video * @conversionRate * @productCost) - (@campaignCostPerVideo * @numberOfVideos)) AS net_profit
FROM
    ChannelData


-- 4.
WHERE
    channel_name IN ('KSI', 'Sidemen', 'JJ Olatunji')


-- 5.
ORDER BY
    net_profit DESC;
```

#### Output

![Most videos](asset/images/youtubers_with_the_most_videos.png)


### 3.  JJ's Youtuber Channel number of views 

#### Calculation breakdown

Campaign idea = Influencer marketing 

a. Sidemen

- Average views per video = 17,340,000 
- Product cost = $5
- Potential units sold per video = 17,340,000  x 2% conversion rate = 346,000 units sold
- Potential revenue per video = 346,000 x $5 = $1,734,000
- Campaign cost (3-month contract) = $130,000
- **Net profit = $1,734,000 - $130,000 = $1,604,000**

b. KSI

- Average views per video = 4,800,000
- Product cost = $5
- Potential units sold per video = 4,800,000 million x 2% conversion rate = 96,000 units sold
- Potential revenue per video = 96,000  x $5 = $480,000
- Campaign cost (3-month contract) = $130,000
- **Net profit = $480,000 - $130,000 = $350,000**

c. JJ Olatunji

- Average views per video = 3,160,000
- Product cost = $5
- Potential units sold per video = 3,160,000 x 2% conversion rate = 63,200 units sold
- Potential revenue per video = 63,200 x $5= $316,000
- Campaign cost (3-month contract) = $130,000
- **Net profit = $316,000 - $130,000 = $186,000**

Best option from category: Sidemen



#### SQL query 
```SQL
/*
# 1. Define variables
# 2. Create a CTE that rounds the average views per video
# 3. Select the columns you need and create calculated columns from existing ones
# 4. Filter results by YouTube channels
# 5. Sort results by net profits (from highest to lowest)
*/



-- 1.
DECLARE @conversionRate FLOAT = 0.02;        -- The conversion rate @ 2%
DECLARE @productCost MONEY = 5.0;            -- The product cost @ $5
DECLARE @campaignCost MONEY = 130000.0;      -- The campaign cost @ $130,000



-- 2.
WITH ChannelData AS (
    SELECT
        channel_name,
        total_views,
        total_videos,
        ROUND(CAST(total_views AS FLOAT) / total_videos, -4) AS avg_views_per_video
    FROM
        youtube_db.dbo.view_uk_youtubers_2024
)


-- 3.
SELECT
    channel_name,
    avg_views_per_video,
    (avg_views_per_video * @conversionRate) AS potential_units_sold_per_video,
    (avg_views_per_video * @conversionRate * @productCost) AS potential_revenue_per_video,
    (avg_views_per_video * @conversionRate * @productCost) - @campaignCost AS net_profit
FROM
    ChannelData


-- 4.
WHERE
    channel_name IN ('KSI', 'Sidemen', 'JJ Olatunji')


-- 5.
ORDER BY
    net_profit DESC;

```

#### Output

![Most views](asset/images/youtubers_with_the_most_views.png)



## Recommendations 

- What do you recommend based on the insights gathered?

1. Sidemen is the best YouTube channel to collaborate with if we want to maximize visibility because this channel has the highest average views per video.

### Potential ROI 

- What ROI do we expect if we take this course of action?
1. Setting up a collaboration deal with Sidemen would generate a net profit of $1,684,000 per video.
2. An influencer marketing contract with KSI can see the client generate a net profit of $430,000 per video.
3. If we go with a product placement campaign with JJ Olatunji, this could generate the client approximately $266,000 per video.


### Action plan
- What course of action should we take and why?

  Based on our analysis, we believe the best channel to advance a long-term partnership deal with to promote the client's products is Sidemen.

We'll have conversations with the marketing client to forecast what they expect from this collaboration. Once we observe that we're hitting the expected milestones, we'll consider potential partnerships with KSI and JJ Olatunji in the future.

- What steps do we take to implement the recommended decisions effectively?


1. Reach out to the teams behind each of these channels, starting with Sidemen.
2. Negotiate contracts within the budget allocated to each marketing campaign.
3. Kick off the campaigns and track their performance against key performance indicators (KPIs).
4. Review how the campaigns have performed, gather insights, and optimize based on feedback from converted customers and audience engagement.
