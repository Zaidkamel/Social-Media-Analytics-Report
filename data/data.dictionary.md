# Social Media Performance Dataset

## Overview
This repository contains the `social_media_performance_data` dataset. It tracks post-level performance metrics across multiple social media platforms, including cost, reach, and user interactions.

## Data Dictionary

| Column Name | Data Type | Description |
| :--- | :--- | :--- |
| **Post_ID** | String | Unique identifier for each social media post. |
| **Post_Date** | Date | The calendar date the content was published. |
| **Post_Time** | Time | The exact time of day the content was published. |
| **Post_Time_Slot** | Categorical | Grouped time of day (e.g., Morning, Midday, Afternoon, Evening). |
| **Platform** | Categorical | The social media platform (TikTok, Instagram, Twitter/X, Facebook). |
| **Content_Type** | Categorical | Format of the post (Video, Image, Carousel, Text). |
| **Campaign** | Categorical | The marketing campaign the post is associated with (e.g., Spring Launch). |
| **Is_Boosted** | Boolean (Yes/No) | Indicates whether paid promotion budget was applied to the post. |
| **Has_Link** | Boolean (Yes/No) | Indicates whether the post caption included a clickable link. |
| **Hashtags_Count** | Integer | The number of hashtags included in the post caption. |
| **Video_Duration_Sec** | Integer | Length of the video in seconds (0 if the format is not a video). |
| **Image_Count** | Integer | Number of images included in the post (e.g., multiple for a carousel). |
| **Caption_Length** | Integer | The total character count of the post's caption. |
| **Production_Cost** | Currency | The cost to create the content, in USD. |
| **Promotion_Budget** | Currency | The ad spend allocated to boost the post, in USD. |
| **Reach** | Integer | The total number of unique users who saw the post. |
| **Frequency** | Float | The average number of times each unique user saw the post. |
| **Impressions** | Integer | The total number of times the post was displayed. |
| **Likes** | Integer | Total number of likes received. |
| **Comments** | Integer | Total number of comments received. |
| **Shares** | Integer | Total number of shares or retweets. |
| **Saves** | Integer | Total number of times users saved or bookmarked the post. |
| **Link_Clicks** | Integer | Total number of clicks on the post's embedded link. |
| **Followers_Gained** | Integer | The number of new followers attributed to this specific post. |
| **Caption_Length_Bucket** | Categorical | Grouping of caption length (Calculated field - see logic below). |
| **Engagements** | Integer | Total sum of all interaction metrics (Calculated field - see logic below). |
| **Is_Viral** | Categorical | Flag indicating high-performing posts (Calculated field - see logic below). |

---

## Calculated Columns

The final three columns in the dataset are generated using formulas rather than raw exported data:

### 1. Caption_Length_Bucket
* **Logic:** Classifies caption length into thresholds (`< 100` = Short, `100–250` = Medium, `> 250` = Long).
* **Table Formula:** `=IF([@[Caption_Length]]<100, "Short", IF([@[Caption_Length]]<=250, "Medium", "Long"))`
* **Cell Formula (Cell O2):** `=IF(A2<100, "Short", IF(A2<=250, "Medium", "Long"))`

### 2. Engagements
* **Logic:** Sums all direct user interaction types (Likes + Comments + Shares + Saves + Link_Clicks).
* **Table Formula:** `=SUM([@[Likes]:[Link_Clicks]])`
* **Cell Formula (Cell P2):** `=SUM(G2:K2)`

### 3. Is_Viral
* **Logic:** Flags content as "Viral" if its total engagements exceed five times the dataset's overall average engagement. Otherwise, it is labeled "Normal".
* **Table Formula:** `=IF([@Engagements] > AVERAGE([Engagements]) * 5, "Viral", "Normal")`
