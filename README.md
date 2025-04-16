
# Youtube-SQL-Analysis
The YouTube SQL Analysis Project aims to solve real-world business questions by analyzing key performance indicators from YouTube data.

## Project Documentation Structure
- ***Introduction***
- ***My Role***
- ***Tools Used***
- ***Problem Statement***
- ***Entity Relationship Diagram***
- ***Dataset Explaination***
- ***Queries, Insights and Recommendation***
- ***Further Analysis***
- ***Conclusion***

 ## Introduction
YouTube is one of the most popular platforms for creators and brands to share content, grow audiences, and earn revenue.

With millions of videos uploaded every day, it's important to track:

- How channels are performing
- How viewers are engaging with content
- Which content strategies drive the most value
- What revenue trends look like across categories

This project uses SQL to explore YouTube data and answer real business questions. The goal is to help creators and marketers make smarter decisions around content planning, audience targeting, and monetization strategies.

## My Role:
As the Data Analyst, I was responsible for:

- Writing efficient SQL queries to answer specific business questions
- Connecting multiple datasets using joins
- Applying subqueries, CTEs, CASE logic, and aggregations for deeper analysis
- Interpreting results to provide actionable insights and recommendations
- Documenting the analysis in a structured and professional format
- Presenting and delivering the project through recorded video and visual slides.

## Tools Used
- **MySQL** – Data analysis and querying
- **Excel** – Initial Data validation
- **GitHub** – Project documentation



## Problem Statement
With millions of videos and channels on YouTube, understanding performance metrics like engagement, views, and revenue is a major challenge for creators, brands, and marketers.

This project focuses on analyzing YouTube data to:

- Identify top-performing channels and videos
- Track audience engagement through likes, comments, and shares
- Understand revenue trends across different categories
- Detect underperforming content and improvement areas

By writing optimized SQL queries, the goal is to generate meaningful insights that support better decision-making in content planning, influencer collaborations, and monetization strategies.

## Entity Relationship Diagram
![image](https://www.digitsndata.in/images/T18.png)

## Dataset Explaination
This project uses a relational YouTube dataset made up of four interconnected tables. Each table captures specific aspects of channel performance and activity:

### Table 1: youtube_channels
This table contains basic details about each YouTube channel.

### Table 2: channel_revenue
This table contains monetary data related to each channel’s earnings.

### Table 3: channel_videos
This table holds data about individual videos uploaded by each channel.

### Table 4: channel_engagement
This table tracks overall engagement trends per video on a channel.

## Queries, Insights and Recommendation

### 1.Get the top 5 most subscribed YouTube channels?
```sql
SELECT 
    channel_name, subscribers
FROM
    youtube_channels
ORDER BY subscribers DESC
LIMIT 5;
```
#### Output
![image](https://github.com/user-attachments/assets/93e8327e-ac50-454b-8c37-4dab44293342)
###### Insights: These channels have a huge audience base, with MrBeast leading the way with 370 million subscribers.
###### Recommendations: Brands looking for influencer collaborations should prioritize these high-subscriber channels.

#### 2. Find the total yearly revenue of all channels?
```sql
WITH revenue_cte AS (
    SELECT 
        yc.channel_name,
        SUM(cr.yearly_revenue) AS total_revenue
    FROM 
        youtube_channels yc JOIN 
        channel_revenue cr ON yc.channel_id = cr.channel_id
    GROUP BY 
        yc.channel_name )
SELECT 
    channel_name,
    CONCAT(ROUND(total_revenue / 1000000, 1), ' M') AS total_year_revenue
FROM 
    revenue_cte
ORDER BY 
    total_revenue DESC;
```
#### Output
![image](https://github.com/user-attachments/assets/0c082746-5568-4d50-8c6c-6ea3040936fe)
###### Insights: MrBeast generates the highest revenue at $60.0M, followed closely by T-Series with $54.0M.
###### Recommendations: Zee Music Company and PewDiePie have the lowest yearly revenue ($9M and $8.4M). They should focus on increasing engagement and monetization

#### 3. Find the video uploaded by 'MrBeast' ? 
```sql
SELECT 
    yc.channel_name, cv.video_title
FROM
    youtube_channels yc JOIN
    channel_videos cv ON yc.channel_id = cv.channel_id
WHERE
    channel_name = 'MrBeast';
```
#### Output
![image](https://github.com/user-attachments/assets/180f76ac-b23f-433b-9abb-0e08daa99fd4)
###### Insights: This type of content aligns with MrBeast’s signature style and attracts massive engagement.
###### Recommendations: Challenge based content is highly engaging and performs well with MrBeast’s audience.

#### 4. Get the average engagement (likes & comments) per video for each channel ?
```sql
SELECT 
    yc.channel_name, 
    CASE 
        WHEN AVG(ce.likes_per_video + ce.comments_per_video) >= 1000000 THEN 
            CONCAT(ROUND(AVG(ce.likes_per_video + ce.comments_per_video) / 1000000, 2), ' M')
        WHEN AVG(ce.likes_per_video + ce.comments_per_video) >= 1000 THEN 
            CONCAT(ROUND(AVG(ce.likes_per_video + ce.comments_per_video) / 1000, 2), ' K')
        ELSE 
            ROUND(AVG(ce.likes_per_video + ce.comments_per_video), 2)
    END AS avg_engagement
FROM 
    youtube_channels yc JOIN 
    channel_engagement ce ON yc.channel_id = ce.channel_id
GROUP BY 
    yc.channel_name
ORDER BY 
    AVG(ce.likes_per_video + ce.comments_per_video) DESC;
```
#### Output
![image](https://github.com/user-attachments/assets/ed3ab586-16b2-445b-8493-9cfdeed92a71)
###### Insights: 
- MrBeast tops engagement with 462K avg likes & comments per video.
- T-Series, Stokes Twins, Cocomelon also show strong interaction.
- Zee Music Company and PewDiePie have lower engagement.
###### Recommendations:
- High performers should keep posting interactive, engaging content.
- Low-engagement channels can add polls, or audience-focused content to boost interaction.

#### 5. List channels that have a NOX score higher than 10 ?
```sql
SELECT 
    channel_name, nox_score
FROM
    youtube_channels
WHERE
    nox_score > 10
ORDER BY nox_score DESC;
```
#### Output
![image](https://github.com/user-attachments/assets/c598b81d-1b5d-4fdd-a042-d287d83ff6b3)
###### Insights: 
- These channels have high NOX scores, indicating strong channel health and potential brand value.
- Zee Music Company leads significantly, showing consistent performance.
###### Recommendations:
- Maintain quality, regular uploads, and community engagement.
- Leverage high NOX for brand deals and collaborations.
- Analyze what drives high NOX and apply it to other content areas.

#### 6. Find the most commented video ?
```sql
SELECT 
    video_title,
    comments
FROM 
    channel_videos
WHERE 
    comments = (
        SELECT 
            MAX(comments)
        FROM 
            channel_videos   );
```
#### Output
![image](https://github.com/user-attachments/assets/701e2c57-c42e-411f-a1c9-81d0d9c84b93)
###### Insights: 
- High number of comments reflects strong audience interaction and content virality.
###### Recommendations:
- Create more challenge-based or interactive content to maintain engagement.
- Encourage comment-driven actions like polls, Q&As, or fan participation for future videos.
  
#### 7. Count the total number of channels in each category ? 
```sql
SELECT 
    category, COUNT(DISTINCT channel_name) AS total_channels
FROM
    youtube_channels
GROUP BY category
ORDER BY total_channels DESC;
```
#### Output
![image](https://github.com/user-attachments/assets/35fb8808-8690-4629-b317-7c5e63e01fa9)
###### Insights: 
- Entertainment dominates the dataset, showing it's the most competitive category.
###### Recommendations:
- Channels in less crowded categories like Education or People & Blogs can stand out by creating specialized, focused content tailored to specific audiences.

#### 8. Search for channel names that start with "M" and end with "t" ?
```sql
SELECT 
    channel_name
FROM
    youtube_channels
WHERE
    channel_name LIKE 'M%'
        AND channel_name LIKE '%t';
```
#### Output
![image](https://github.com/user-attachments/assets/244877fa-7981-4a1e-9c86-818fbcb33705)
###### Insights: Only one channel matches the pattern — MrBeast. Such queries help filter specific naming patterns for reporting or segmentation.

#### 9. Find videos uploaded between '2024-03-01' AND '2024-08-01' ?
```sql
SELECT 
    video_title
FROM
    channel_videos
WHERE
    upload_date BETWEEN '2024-03-01' AND '2024-08-01';
```
#### Output
![image](https://github.com/user-attachments/assets/0795ed62-bbdf-4f4d-8762-a1f93edc8d59)
###### Insights: Only 3 videos afe uploaded within the period
###### Recommendations: Focus promotional efforts and audience engagement strategies around these recent uploads to maximize visibility and retention.

#### 10. Find the top 3 most liked videos on YouTube ?
```sql
SELECT 
    cv.video_title,
    CONCAT(ROUND(cv.likes / 1000, 0), ' K') AS video_likes
FROM 
    channel_videos cv
ORDER BY 
    cv.likes DESC
LIMIT 3;
```
#### Output
![image](https://github.com/user-attachments/assets/57864228-4554-4ce9-8d49-2814995c9393)
###### Insights: 
- Epic Challenge Video leads with 500K likes, showing strong viewer approval and engagement.
- Best Bollywood Songs and Crazy Stunts Compilation also perform well, reflecting audience interest in music and thrill-based content.
###### Recommendations: Encourage viewers to like and share to maintain high engagement momentum.

 ## Further Analysis

 #### Which YouTube channel shows the lowest overall engagement across likes, comments, and shares per video?
 ```sql
SELECT 
    yc.channel_name,
    CONCAT(ROUND((ce.likes_per_video + ce.comments_per_video + ce.shares_per_video) / 1000,
                    0),
            ' K') AS total_engagement
FROM
    youtube_channels yc JOIN
    channel_engagement ce ON yc.channel_id = ce.channel_id
ORDER BY (ce.likes_per_video + ce.comments_per_video + ce.shares_per_video) ASC
LIMIT 1;
```
#### Output
![image](https://github.com/user-attachments/assets/f91dc75f-9b60-4777-8c3f-1b3b2c7f2129)
###### Insights: Zee Music Company has the lowest overall engagement at 57K per video (likes + comments + shares), indicating lower audience interaction compared to other channels.
###### Recommendations:
- Increase audience interaction through polls, comments, and CTAs (call-to-actions).
- Collaborate with trending artists or influencers to boost visibility and engagement.
- Experiment with interactive or behind-the-scenes content to spark more viewer interest.

##### Which videos have outperformed their respective channel’s average view count?
```sql
WITH converted_views AS (
    SELECT 
        yc.channel_id,
        yc.channel_name,
        cv.views,
        CASE
            WHEN yc.avg_views LIKE '%M' THEN REPLACE(yc.avg_views, 'M', '') * 1000000
            WHEN yc.avg_views LIKE '%K' THEN REPLACE(yc.avg_views, 'K', '') * 1000
            ELSE yc.avg_views
        END AS avg_views_numeric
    FROM 
        youtube_channels yc JOIN 
        channel_videos cv ON yc.channel_id = cv.channel_id
)

SELECT DISTINCT 
    channel_name
FROM 
    converted_views
WHERE 
    views > avg_views_numeric;
```
#### Output
![image](https://github.com/user-attachments/assets/ebddcd92-96c4-4f7f-8637-696be13405ea)
###### Insights: Videos from T-Series, Cocomelon - Nursery Rhymes, SET India, and Vlad and Niki have outperformed their respective channel’s average view count, showing stronger-than-usual viewer interest.
###### Recommendations:
- Analyze the content style, title, and timing of these successful videos to replicate what worked.
- Promote similar formats or themes in future uploads.

#### What is the average engagement (likes, comments, and shares per video) across different YouTube content categories?
```sql
SELECT 
    yc.category,
    CONCAT(ROUND(AVG(ce.likes_per_video) / 1000, 2), ' K') AS avg_likes,
    CONCAT(ROUND(AVG(ce.comments_per_video) / 1000, 2), ' K') AS avg_comments,
    CONCAT(ROUND(AVG(ce.shares_per_video) / 1000, 2), ' K') AS avg_shares
FROM 
    youtube_channels yc JOIN 
    channel_engagement ce ON yc.channel_id = ce.channel_id
GROUP BY 
    yc.category
ORDER BY 
    AVG(ce.likes_per_video) DESC;
```
#### Output
![image](https://github.com/user-attachments/assets/353c273f-6506-40a3-96d0-08fb5db84747)
###### Insights: 
- Entertainment leads with the highest average likes (229K), reflecting its broad appeal.
- People & Blogs and Education have strong comments and shares, showing active audience interaction.
- Gaming has the lowest engagement across all metrics.  
###### Recommendations:
- Channels in Entertainment should continue leveraging viral and relatable content.
- People & Blogs and Education creators can boost visibility by promoting community engagement and feedback loops.
- Gaming channels should experiment with interactive content (e.g., live Q&A, polls) to improve viewer interaction.

#### Which content category generates the highest average yearly revenue per channel?
```sql
SELECT 
    yc.category, 
    CONCAT(ROUND(SUM(cr.yearly_revenue) / COUNT(DISTINCT yc.channel_id) / 1000000, 2), ' M') AS avg_revenue_per_channel
FROM 
    youtube_channels yc JOIN 
    channel_revenue cr ON yc.channel_id = cr.channel_id
GROUP BY 
    yc.category
ORDER BY 
    SUM(cr.yearly_revenue) / COUNT(DISTINCT yc.channel_id) DESC;
```
#### Output
![image](https://github.com/user-attachments/assets/897a096a-8510-4739-b84b-b72897912322)
###### Insights: 
- Music channels generate the highest average revenue (31.5M) per channel.
- Entertainment and Education follow with 21M and 18M respectively.
- People & Blogs and Gaming are at the lower end, under 10M.
###### Recommendations:
- Music creators should capitalize on their high revenue potential through sponsorships and licensing deals.
- Gaming and Blogs channels can explore additional revenue streams like merchandise, memberships, or brand collaborations to boost earnings.

#### Which YouTube channels have high total engagement but a low NOX score?
```sql
SELECT 
    yc.channel_name,
    CONCAT(ROUND((ce.likes_per_video + ce.comments_per_video + ce.shares_per_video) / 1000, 2), ' K') AS total_engagement,
    yc.nox_score
FROM 
    youtube_channels yc JOIN 
    channel_engagement ce ON yc.channel_id = ce.channel_id
WHERE 
    yc.nox_score < 10
ORDER BY 
    (ce.likes_per_video + ce.comments_per_video + ce.shares_per_video) DESC;
```
#### Output
![image](https://github.com/user-attachments/assets/ca525a67-2876-46d3-b9da-6904aba8bb00)
###### Insights: 
- Channels like MrBeast, Cocomelon, and PewDiePie show high total engagement but have a low NOX score (<1).
- This indicates strong viewer interaction but lower brand value or monetization potential as per NOX metrics.
###### Recommendations:
- These creators can optimize brand collaborations and enhance monetization strategies to align with their audience engagement.
- They should focus on audience targeting, brand storytelling, and sponsorship pitches to improve their NOX rating.

 ## Conclusion

 - #### Performed detailed SQL analysis on YouTube channel metrics like views, engagement, and revenue.

- #### Identified top-performing channels and most engaging content across various categories.

- #### Highlighted channels with high engagement but low NOX score for optimization.

- #### Revealed which content types drive higher revenue and audience interaction.

- #### Demonstrated how SQL can be used to extract actionable insights from complex datasets.

- #### Recommendations help channels improve content strategy, engagement, and monetization.



 
