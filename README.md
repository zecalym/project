# social media usage

# Table of contents
- [project overview](#project-overview)
- [Data source](#data-source)
- [Tools](#tools)
- [Data Cleaning/Preparation](#data-cleaning/preparation)
- [Exploratory Data Analysis](#Exploratory`Data-Analysis)
- [Result/Finding](#reuslt/finding)
- [Recommedation](#Recommedation)


### project Overview

This project was created to show the usage of social media, which gender uses the media the most and what media is used the most Also, it shows the age of people who are liking consuming the social media. By analyzing this data, we seek to identify trends, make data- driven recommendation and make deeper understanding on the social media usage.

### Data Source

The primary data use for this analysis is from Kaggle
Containing detail of gender usage on the "social media csv". 


### Tools
- My Sql workbench - Data cleaning 
- Power Bi - Creating reports

 ### Data Cleaning/Preparation

 
 in the initial data preparation we prepare the data task
 1. Date location and inspectation.
 2. Create another table and change the names of the column. 
 3. Check for duplicate values.
 4. Incorrect spelling in the rows.
 5. Check for upper and lower case.
 6. Check data type.
 7. Remove unwanted columns.

 ### Exploratory Data Analysis
 - which social media is used the most?
 - what are the age of the gender using the social media?
 - which gender use the media the most?
 - sum of post in each social media?

### Data Analysis
```sql
CREATE TABLE `test1` (
  `User_ID` int DEFAULT NULL,
  `Age` int DEFAULT NULL,
  `Gender` text,
  `Platform` text,
  `Daily_Usage_Time_minutes` int DEFAULT NULL,
  `Posts_Per_Day` int DEFAULT NULL,
  `Likes_Received_Per_Day` int DEFAULT NULL,
  `Comments_Received_Per_Day` int DEFAULT NULL,
  `Messages_Sent_Per_Day` int DEFAULT NULL,
  `Dominant_Emotion` text
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

insert into test1
select * from test;

SELECT *
FROM zec.test1;

/* check for Duplicate*/

select*,
row_number() over(
partition by user_id) duplicate_number
from test1;

CREATE TABLE `social_media_reaction` (
  `User_ID` int DEFAULT NULL,
  `Age` int DEFAULT NULL,
  `Gender` text,
  `Platform` text,
  `Daily_Usage_Time_minutes` int DEFAULT NULL,
  `Posts_Per_Day` int DEFAULT NULL,
  `Likes_Received_Per_Day` int DEFAULT NULL,
  `Comments_Received_Per_Day` int DEFAULT NULL,
  `Messages_Sent_Per_Day` int DEFAULT NULL,
  `Dominant_Emotion` text,
  `duplicate_number` int
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

insert into social_media_reaction
select*,
row_number() over(
partition by user_id) duplicate_number
from test1;

select *
from social_media_reaction;

select user_id, duplicate_number
from social_media_reaction
where duplicate_number >1;

delete 
from social_media_reaction
where duplicate_number >1;

/*check for incorrect spelling in the rows*/

select *
from social_media_reaction;

select distinct gender
from social_media_reaction;

select gender
from social_media_reaction
where gender like "male%";

update social_media_reaction
set gender = "male"
where gender like "marie%";

/* check for proper case in the rows*/

 select concat(upper(left("male",1)),substring("male",2)) as proper_case
 from social_media_reaction
 where gender like "male%";

update social_media_reaction
set gender = concat(upper(left("male",1)),substring("male",2))
where gender like "male%";

/* check data types */
 
 select*
 from information_schema.columns
 where table_schema = "SOCIAL_MEDIA_REACTION";
 
select *
from social_media_reaction;

/* remove unwanted columns */

alter table social_media_reaction
drop duplicate_number;

select *
from social_media_reaction;

/* data quality test 
1 duplicate. !!!passed
2 incorrect words. !!!passed
3 column must be 10 !!!passed
4 rowm must be 97 !!!passed*/

select user_id, count(*)
from social_media_reaction
group by user_id
having count(*) >1;

select distinct gender
from social_media_reaction;

select *
from social_media_reaction;

select count(*)
from social_media_reaction;
```


### Result/Finding
1. The analysis result as follow;
2. Male gender has the most usage with daily count usage of 44 Followed by Non binary and female.
3. Age of the gender who consume the social media the mostboth male and female are age 28.
4. Social media used the most are Instagram and Twitter.

### Recommedation
base on my analysis, we recommed the following action;
1. Online investor/company should focus on the gender within the age 27- 28.
2. Advert to push new product should be focus on instagram and twitter.
3. Advert should be focus more on male gender than female.

