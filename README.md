# Data Portfolio: Excel to Power BI 


![excel-to-powerbi-animated-diagram](assets/images/kaggle_to_powerbi.gif)




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

The Head of Marketing wants to find out who the top YouTubers are in 2024 to decide on which YouTubers would be best to run marketing campaigns throughout the rest of the year.


- What is the ideal solution? 

To create a dashboard that provides insights into the top UK YouTubers in 2024 includes their 
- subscriber count
- total views
- total videos, and
- engagement metrics

This will help the marketing team decide which YouTubers to collaborate with for their marketing campaigns.

## User story 

As the Head of Marketing, I want to use a dashboard that analyses YouTube channel data in the UK. 

This dashboard should allow me to identify the top-performing channels based on metrics like subscriber base and average views. 

With this information, I can make more informed decisions about which Youtubers are right to collaborate with, maximizing how effective each marketing campaign is.


# Data source 

- What data is needed to achieve our objective?

We need data on the top UK YouTubers in 2024 including their 
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
2. Which 3 channels have uploaded the most videos?
3. Which 3 channels have the most views?
4. Which 3 channels have the highest average views per video?
5. Which 3 channels have the highest views per subscriber ratio?
6. Which 3 channels have the highest subscriber engagement rate per video uploaded?

For now, these are some of the questions we need to answer, this may change as we progress down our analysis. 


## Dashboard mockup

- What should it look like? 

Some of the data visuals that may be appropriate in answering our questions include:

1. Table
2. Treemap
3. Scorecards
4. Horizontal bar chart 




![Dashboard-Mockup](assets/images/dashboard_mockup.png)
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
3. Load the data into SQL Server
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
2. The first column contains the channel ID with what appears to be channel IDS, which are separated by a @ symbol - we need to extract the channel names from this.
3. Some of the cells and header names are in a different language - we need to confirm if these columns are needed, and if so, we need to address them.
4. We have more data than we need, so some of these columns would need to be removed





## Data cleaning 
- What do we expect the clean data to look like? (What should it contain? What constraints should we apply to it?)

The aim is to refine our dataset to ensure it is structured and ready for analysis. 

The cleaned data should meet the following criteria and constraints:

- Only relevant columns should be retained.
- All data types should be appropriate for the contents of each column.
- No column should contain null values, indicating complete data for all records.

Below is a table outlining the constraints on our cleaned dataset:

| Property | Description |
| --- | --- |
| Number of Rows | 100 |
| Number of Columns | 4 |

And here is a tabular representation of the expected schema for the clean data:

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



```sql
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

```sql
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
```sql
/*
# Count the total number of records (or rows) in the SQL view
*/

SELECT
    COUNT(*) AS no_of_rows
FROM
    view_uk_youtubers_2024;

```

![Row count check](assets/images/row_check.png)



## Column count check
### SQL query 
```sql
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
![Column count check](assets/images/column_count.png)

## Data type check
### SQL query 
```sql
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
![Data type check](assets/images/datatype.png)



## Results

- What does the dashboard look like?

![GIF of Power BI Dashboard](assets/images/powerbi_dashboard.png)

This shows the Top UK Youtubers in 2024 so far.

## DAX Measures

### 1. Total Subscribers (M)
```sql
Total Subscribers (M) = 
VAR million = 1000000
VAR sumOfSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR totalSubscribers = DIVIDE(sumOfSubscribers,million)

RETURN totalSubscribers

```

### 2. Total Views (B)
```sql
Total Views (B) = 
VAR billion = 1000000000
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR totalViews = ROUND(sumOfTotalViews / billion, 2)

RETURN totalViews

```

### 3. Total Videos
```sql
Total Videos = 
VAR totalVideos = SUM(view_uk_youtubers_2024[total_videos])

RETURN totalVideos

```

### 4. Average Views Per Video (M)
```sql
Average Views per Video (M) = 
VAR sumOfTotalViews = SUM(view_uk_youtubers_2024[total_views])
VAR sumOfTotalVideos = SUM(view_uk_youtubers_2024[total_videos])
VAR  avgViewsPerVideo = DIVIDE(sumOfTotalViews,sumOfTotalVideos, BLANK())
VAR finalAvgViewsPerVideo = DIVIDE(avgViewsPerVideo, 1000000, BLANK())

RETURN finalAvgViewsPerVideo 

```


### 5. Subscriber Engagement Rate
```sql
Subscriber Engagement Rate = 
VAR sumOfTotalSubscribers = SUM(view_uk_youtubers_2024[total_subscribers])
VAR sumOfTotalVideos = SUM(view_uk_youtubers_2024[total_videos])
VAR subscriberEngRate = DIVIDE(sumOfTotalSubscribers, sumOfTotalVideos, BLANK())

RETURN subscriberEngRate 

```


### 6. Views per subscriber
```sql
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
2. Which 3 channels have uploaded the most videos?
3. Which 3 channels have the most views?
4. Which 3 channels have the highest average views per video?
5. Which 3 channels have the highest views per subscriber ratio?
6. Which 3 channels have the highest subscriber engagement rate per video uploaded?


### 1. Who are the top 10 YouTubers with the most subscribers?

| Rank | Channel Name         | Subscribers (M) |
|------|----------------------|-----------------|
| 1    | NoCopyrightSounds    | 33.89           |
| 2    | DanTDM               | 29.19           |
| 3    | Dan Rhodes           | 26.99           |
| 4    | Miss Katy            | 25.29           |
| 5    | KSI                  | 24.99           |
| 6    | Mister Max           | 24.89           |
| 7    | Dua Lipa             | 23.99           |
| 8    | Jelly                | 23.49           |
| 9    | Colinfurze           | 20.49           |
| 10   | Ali-A                | 19.49           |


### 2. Which 3 channels have uploaded the most videos?

| Rank | Channel Name    | Videos Uploaded |
|------|-----------------|-----------------|
| 1    | Boomerang UK    | 186,272         |
| 2    | More Emily      | 57,755          |
| 3    | Sing King       | 43,211          |



### 3. Which 3 channels have the most views?


| Rank | Channel Name | Total Views (B) |
|------|--------------|-----------------|
| 1    | DanTDM       | 20.08           |
| 2    | Dan Rhodes   | 19.13           |
| 3    | Mister Max   | 16.39           |


### 4. Which 3 channels have the highest average views per video?

| Channel Name | Average Views per Video (M) |
|--------------|-----------------|
| 24 News HD   | 34.63           |
| Slogo        | 6.28            |
| Dua Lipa     | 4.59            |


### 5. Which 3 channels have the highest views per subscriber ratio?

| Rank | Channel Name       | Views per Subscriber        |
|------|-----------------   |---------------------------- |
| 1    | Dumori Bay         | 1204.55                     |
| 2    | ARPO The Robot     | 1064.27                     |
| 3    | Awakening Music    | 1042.51                     |



### 6. Which 3 channels have the highest subscriber engagement rate per video uploaded?

| Rank | Channel Name    | Subscriber Engagement Rate  |
|------|-----------------|---------------------------- |
| 1    | 24 News HD      | 351,000                     |
| 2    | Slogo           | 111,458.33                  |
| 3    | Dua Lipa        | 77,922.07                   |




### Notes

For this analysis, we'll prioritize analysing the metrics that are important in generating the expected ROI for our marketing client, which are the YouTube channels wuth the most 

- subscribers
- total views
- videos uploaded



## Validation 

### 1. Youtubers with the most subscribers 

#### Calculation breakdown

Campaign idea = product placement 

1. **NoCopyrightSounds**
- Average views per video = 6.60 million
- Product cost = $5
- Potential units sold per video = 6.60 million x 2% conversion rate = 121,200 units sold
- Potential revenue per video = 12,200 x $5 = $606,000
- Campaign cost (one-time fee) = $50,000
- **Net profit = $660,000 - $50,000 = $610,000**

b. **DanTDM**

- Average views per video = 5.37 million
- Product cost = $5
- Potential units sold per video = 5.37 million x 2% conversion rate = 107,400 units sold
- Potential revenue per video = 107,400 x $5 = $537,000
- Campaign cost (one-time fee) = $50,000
- **Net profit = $537,000 - $50,000 = $487,000**

c. **Dan Rhodes**

- Average views per video = 11.4 million
- Product cost = $5
- Potential units sold per video = 11.4 million x 2% conversion rate = 228,000 units sold
- Potential revenue per video = 228,000 x $5 = $1,140,000
- Campaign cost (one-time fee) = $50,000
- **Net profit = $1,140,000 - $50,000 = $1,090,000**


Best option from category: Dan Rhodes

#### SQL query 

```sql
/* 

# 1. Define variables 
# 2. Create a CTE that rounds the average views per video 
# 3. Select the column you need and create calculated columns from existing ones 
# 4. Filter Results by Youtube channels
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
    channel_name in ('NoCopyrightSounds', 'DanTDM', 'Dan Rhodes')    


-- 5.  
ORDER BY
	net_profit DESC

```

#### Output
![Most subsc](assets/images/youtube_most_sub.png)

### 2. Youtubers with the most videos uploaded

### Calculation breakdown 

Campaign idea = sponsored video series  

a. **24 News HD**
- Average views per video = 346,370,000
- Product cost = $5
- Potential units sold per video = 346,370,000 x 2% conversion rate = 6,927,400 units sold
- Potential revenue per video = 6,927,400 x $5= 34,637,000
- Campaign cost (11-videos @ $5,000 each) = $55,000
- Total revenue (for 11 videos) = $34,637,000 × 11 = $380,007,000
- **Net profit = $380,007,000 - $55,000 = $379,952,000**

b. **Slogo**

- Average views per video = 62,840,000
- Product cost = $5
- Potential units sold per video = 62,840,000 x 2% conversion rate = 1,256,800 units sold
- Potential revenue per video = 1,256,800 x $5= $6,284,000
- Campaign cost (11-videos @ $5,000 each) = $55,000
- Total revenue (for 11 videos) = $6,284,000 × 11 = $69,124,000
- **Net profit = $6,284,000 - $55,000 = $6,229,000**

c. **Dua Lipa**

- Average views per video = 45,910,000
- Product cost = $5
- Potential units sold per video = 45,910,000 x 2% conversion rate = 918,200 units sold
- Potential revenue per video = 918,200 x $5= $4,591,000
- Campaign cost (11-videos @ $5,000 each) = $55,000
- Total revenue (for 11 videos) = $4,591,000 × 11 = $50,501,000
- **Net profit = $69,124,000 - $55,000 = $69,069,000**


Best option from category: 24 News HD

#### SQL query 
```sql
/* 
# 1. Define variables
# 2. Create a CTE that rounds the average views per video
# 3. Select the columns you need and create calculated columns from existing ones
# 4. Filter Results by YouTube channels
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

ORDER BY
    5 DESC;
```

#### Output
![Most videos](assets/images/most_vid_upload.png)

### 3.  Youtubers with the most views 

#### Calculation breakdown

Campaign idea = Influencer marketing 

a. **24 News HD**

- Average views per video = 346,370,000
- Product cost = $5
- Potential units sold per video = 346,370,000 x 2% conversion rate = 6,927,400 units sold
- Potential revenue per video = 6,927,400 x $5 = $34,637,000
- Campaign cost (3-month contract) = $130,000
- **Net profit = $34,637,000 - $130,000 = $34,507,000**

b. **Slogo**

- Average views per video = 62,840,000
- Product cost = $5
- Potential units sold per video = 62,840,000 x 2% conversion rate = 1,256,800 units sold
- Potential revenue per video = 1,256,800 x $5 = $6,284,000
- Campaign cost (3-month contract) = $130,000
- **Net profit = $6,284,000 - $130,000 = $6,154,000**

c. **Dua Lipa**

- Average views per video = 45,910,000
- Product cost = $5
- Potential units sold per video = 45,910,000 x 2% conversion rate = 918,200 units sold
- Potential revenue per video = 918,200 x $5 = $4,591,000
- Campaign cost (3-month contract) = $130,000
- **Net profit = $4,591,000 - $130,000 = $4,461,000**

Best option from category: 24 News HD

#### SQL query 
```sql
/*
# 1. Define variables
# 2. Create a CTE that rounds the average views per video
# 3. Select the columns you need and create calculated columns from existing ones
# 4. Sort results by net profits (from highest to lowest)
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
ORDER BY
    2 DESC;

```

#### Output

![Most views](assets/images/most_views.png)

## Discovery

- What did we learn?

**Final Analysis & Recommendation**
Based on the calculated net profits across different campaign types, here is a breakdown of findings and final recommendations:

**Key Findings**
Best Overall ROI: Sponsored Video Series with 24 News HD
Net profit: $379,952,000 (for 11 videos)
Extremely high reach and potential return
Cost-effective compared to other campaigns
Recommendation: Prioritize 24 News HD for a sponsored video series, as it yields the highest potential revenue and ROI.

Strong ROI from Influencer Marketing with 24 News HD
Net profit: $34,507,000
Lower upfront cost ($130,000) vs. the sponsored video series
Best for a more flexible and shorter campaign
Recommendation: If the budget is limited, use influencer marketing with 24 News HD instead of a fully sponsored video series.

Product Placement Campaign (Dan Rhodes Performs Best)
Net profit: $1,090,000 (best among the product placement options)
Ideal for highly engaged audiences but lower reach than the 24 News HD campaigns
Recommendation: Use product placement with Dan Rhodes if the goal is to focus on an engaged niche audience rather than massive reach.

Dua Lipa and Slogo Have Lower ROI
Net profit for Dua Lipa (Influencer Marketing): $4,461,000
Net profit for Slogo (Sponsored Video Series): $6,229,000
Both are profitable, but returns are much lower than 24 News HD
Recommendation: If the budget is tight, avoid these options unless they specifically align with your target audience.

**Final Recommendation:** How to Allocate Budget?
-Best ROI Approach (If Budget is Unlimited)
-Invest in a Sponsored Video Series with 24 News HD (Highest ROI)
-Add Product Placement with Dan Rhodes for additional niche engagement

**Balanced Approach (If Budget is Moderate)**
-Use Influencer Marketing with 24 News HD
-Complement it with NoCopyrightSounds (or DanTDM) for a diversified audience reach

**Low Budget Approach (If Budget is Limited)**
-Focus on Product Placement with Dan Rhodes (Best low-cost option)
-Test a single video sponsorship before committing to a full campaign

**Final Thoughts**
24 News HD offers the best ROI across multiple campaign types.
Dan Rhodes is the best product placement option for smaller campaigns.
Avoid Dua Lipa and Slogo unless targeting specific niches.
Consider testing a small campaign before scaling up.


### Action Plan
**Action Plan for YouTube Marketing Campaign**
Based on the analysis, this action plan will help execute the most profitable and cost-effective YouTube influencer campaign.

**Step 1: Define Campaign Goals**
Objective: Maximize ROI through targeted influencer marketing
KPIs: Sales conversion rate (target: 2%+)
          Total revenue vs. campaign cost
          Engagement rate (likes, comments, shares)
          Brand awareness (growth in followers & website traffic)

**Step 2: Select the Best YouTube Influencers**
-Priority 1: 24 News HD (Sponsored Video Series)
-11-video series ($55,000)
-Expected revenue: $380M+
-Best for: Massive reach, high ROI, broad audience

-Priority 2: Dan Rhodes (Product Placement)
-Single video sponsorship ($50,000)
-Expected revenue: $1.09M
-Best for: High engagement, niche audience, cost-effective approach

-Backup: NoCopyrightSounds & DanTDM (if budget allows)
-Low-cost, engaged audience, strong music niche

**Step 3: Contact Influencers & Negotiate Deals**
Who to reach out to:
-Influencer Management Agencies (for big YouTubers like 24 News HD & Dua Lipa)
-Direct Email/DM for smaller creators (Dan Rhodes, DanTDM, NoCopyrightSounds)

Negotiation strategy:
-Bulk Discounts (for multiple videos)
-Affiliate Deals (lower upfront fees in exchange for revenue share)
-Performance-based pricing (pay more if sales exceed targets)

**Step 4: Develop Creative Content & Messaging**
-Video scripts & key talking points
-Highlight product benefits clearly
-Create a sense of urgency (limited-time offers, discounts)
-Call-to-action (CTA) linking to purchase page

Example CTA:
"Click the link below to get 20% OFF today! 🎉 Don’t miss out!"

**Step 5: Launch & Promote Campaign**
-Pre-launch teasers (social media, influencer posts)
-YouTube ads boosting (if extra budget is available)
-Monitor audience reactions & engagement
-Leverage UGC (User-Generated Content) for more credibility

**Step 6: Track Performance & Optimize**
-Monitor KPIs:

-Conversion rate 
-Total sales & revenue 
-Engagement (likes, shares, comments)
-Customer feedback 
-Use Analytics Tools:

-Google Analytics
-YouTube Studio
-UTM tracking for sales attribution
-Optimize based on performance:

-If high engagement → Scale up the campaign
-If low conversion → Adjust messaging & CTAs

**Step 7: Scale Successful Strategies**
-Double down on best-performing influencers
-Expand to TikTok, Instagram, or Shorts for more reach
-Offer influencers affiliate links for long-term partnerships

Final Recommendations
-Start with 24 News HD + Dan Rhodes (Best ROI)
-Monitor and optimize for higher conversion rates
-Expand to other platforms if the campaign is successful
