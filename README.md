# SQL-CoolTShirts-Touch-Attribution-Query

# Project Description:

## Introduction

CoolTShirts (CTS) is a trendy online retailer that specializes in selling a wide variety of shirts, with one primary requirement – they must be T-shaped and, of course, cool. To boost their online presence and increase sales, CTS has initiated several marketing campaigns in recent times. These campaigns are designed to attract visitors to the website and encourage them to make purchases. To better understand the effectiveness of these marketing initiatives, CTS is keen on implementing touch attribution analysis. By mapping the customer journey from the initial website visit to the final purchase, they aim to optimize their marketing strategies and allocate resources more efficiently.

## Data Overview
### page_visits Table: 
page_name, timestamp, user_id, utm_campaign, utm_source

## Project Objectives


-   **Data Collection:** Data from CTS's website and marketing channels will be collected, including details on website visits, marketing campaign interactions, and conversion events.
    
-   **Data Preparation:** The collected data will be cleaned and organized, ensuring that it is in a format suitable for analysis.
    
-   **Touch Attribution Models:** Various attribution models, such as First Touch, Last Touch, Linear, Time Decay, and U-Shaped, will be applied to the data to understand the influence of different touchpoints.
    
-   **Customer Journey Mapping:** The customer journey will be examined, revealing the paths that customers take before making a purchase.
    
-   **Optimization Strategies:** Recommendations will be formulated based on the insights from the analysis. These may include adjusting marketing budgets, improving targeting, or experimenting with new channels.


### My work:



    

    
      
    -- 1
    -- How many campaigns and sources does CoolTShirts use? Which source is used for each campaign?
    -- Number of campaigns:
    
    SELECT  COUNT(DISTINCT utm_campaign)  AS  'campaigns_count'
    
    FROM page_visits;
![enter image description here](https://i.ibb.co/nfwVm7r/campaigns-count.png)
    
      
    
    -- Number of sources:
    
    SELECT  COUNT(DISTINCT utm_source)  AS  'sources_count'
    
    FROM page_visits;
![enter image description here](https://i.ibb.co/x5ydW8c/sources-count.png)
    
      
    
    -- Which campaigns are running on which sources:
    
    SELECT  DISTINCT utm_campaign,
    
    utm_source
    
    FROM page_visits;
![enter image description here](https://i.ibb.co/5v37317/3.png)
    
      
    
    -- 2
    
    -- What pages are on the CoolTShirts website?
    
    SELECT  DISTINCT page_name
    
    FROM page_visits;
![enter image description here](https://i.ibb.co/Z19nSjM/4.png)
    
      
    
    -- Write a query to find users' first touch on the website. How many first touches is each campaign responsible for?
    
    WITH first_touch AS  (
    
    SELECT user_id,
    
    MIN(timestamp)  as first_touch_at
    
    FROM page_visits
    
    GROUP  BY user_id)
    
    SELECT ft.user_id,
    
    ft.first_touch_at,
    
    pv.utm_source,
    
    pv.utm_campaign,
    
    COUNT(utm_campaign)  AS  'users_first_touch'
    
    FROM first_touch ft
    
    JOIN page_visits pv
    
    ON ft.user_id = pv.user_id
    
    AND ft.first_touch_at = pv.timestamp
    
    GROUP  BY  4
    
    ORDER  BY  5;
![enter image description here](https://i.ibb.co/4j3M55N/first-touch-at.png)
    
      
    
    -- 4
    
    -- Write a query to find users' last touch on the website. How many last touches is each campaign responsible for?
    
    WITH last_touch AS  (
    
    SELECT user_id,
    
    MAX(timestamp)  as last_touch_at
    
    FROM page_visits
    
    GROUP  BY user_id)
    
    SELECT lt.user_id,
    
    lt.last_touch_at,
    
    pv.utm_source,
    
    pv.utm_campaign,
    
    COUNT(utm_campaign)  AS  'users_last_touch'
    
    FROM last_touch lt
    
    JOIN page_visits pv
    
    ON lt.user_id = pv.user_id
    
    AND lt.last_touch_at = pv.timestamp
    
    GROUP  BY  4
    
    ORDER  BY  5;
![enter image description here](https://i.ibb.co/RPLmw0x/last-touch-at.png)
    
      
    
    -- 5
    
    -- How many users made a purchase?
    
    SELECT  COUNT(DISTINCT user_id)  AS  'total_purchases'
    
    FROM page_visits
    
    WHERE page_name = '4 - purchase';
![enter image description here](https://i.ibb.co/S7691xB/total-ourchases.png)
    
      
    
    -- 6
    
    -- How many last touches on the purchase page is each campaign responsible for?
    
    WITH last_touch AS  (
    
    SELECT user_id,
    
    MAX(timestamp)  as last_touch_at
    
    FROM page_visits
    
    WHERE page_name = '4 - purchase'
    
    GROUP  BY user_id)
    
    SELECT lt.user_id,
    
    lt.last_touch_at,
    
    pv.utm_source,
    
    pv.utm_campaign,
    
    COUNT(utm_campaign)  AS  'last_touch_count'
    
    FROM last_touch lt
    
    JOIN page_visits pv
    
    ON lt.user_id = pv.user_id
    
    AND lt.last_touch_at = pv.timestamp
    
    GROUP  BY  4
    
    ORDER  BY  5;
![enter image description here](https://i.ibb.co/C8JwdZn/final-one.png)
      

-- Given the data in this project, if CoolTShirts were to re-invest in five campaigns, they should pick weekly-newsletter, retargetting-ad on Facebook, retargetting-campaign email, paid-search, and either ten-crazy-cool-tshirts-facts or getting-to-know-cool-tshirts article as these campaigns resulted in the most purchases.


