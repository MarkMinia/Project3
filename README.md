# SQL Database Creation: Anime Recommendations 2020
#### [Original CSV](https://github.com/MarkMinia/Project3/blob/main/Dataset/anime.csv)
#### [Cleaned CSV](https://github.com/MarkMinia/Project3/blob/main/Dataset/anime_cleaned.csv)

#### [Kaggle Link](https://www.kaggle.com/datasets/hernan4444/anime-recommendation-database-2020)


##### The dataset downloaded from Kaggle contained over 17,562 rows and 35 columns. When going through the dataset in Excel, I performed the following:
- ##### Adjust id column series to be in order
- ##### Replace 'Unknown' and blank cells with 'NULL' for text columns
- ##### Replace blank cells with 0 for numeric columns
- ##### Identify cells with lists seperated by commas and create individual columns for each value
- ##### Change the dates to be the same format
- ##### Change column labels to include underscores in preperation for SQL import

##### Note: Some tables may be too large to view in Github. To view, download the file and open on your desktop. 

##### Before importing the file, I needed to create the table and specify the columns and datatypes. When attempting this through the Import Wizard, there were issues of not capturing all 17,000+ rows, so this step was done manually. The table is structured as such in preperation to normalize the data to 3NF form. It will essentially serve as the base table in which I would query from to create the actual tables that can be used for analysis.    
```sql
CREATE TABLE anime_cleaned 
(MAL_ID INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
title VARCHAR(100),		
score DEC(3, 2),	
genres_0 VARCHAR(50),	
genres_1 VARCHAR(50),	
genres_2 VARCHAR(50),	
genres_3 VARCHAR(50),	
genres_4 VARCHAR(50),	
genres_5 VARCHAR(50),	
genres_6 VARCHAR(50),	
genres_7 VARCHAR(50),	
genres_8 VARCHAR(50),	
genres_9 VARCHAR(50),	
genres_10 VARCHAR(50),	
genres_11 VARCHAR(50),	
genres_12 VARCHAR(50),	
media_type VARCHAR(50),
episodes INT,	
aired VARCHAR(50),	
premiered VARCHAR(50),	
producers VARCHAR(50),	
producers_0	VARCHAR(50),
producers_1	VARCHAR(50),
producers_2	VARCHAR(50),
producers_3	VARCHAR(50),
producers_4	VARCHAR(50),
producers_5	VARCHAR(50),
producers_6	VARCHAR(50),
producers_7	VARCHAR(50),
producers_8	VARCHAR(50),
producers_9	VARCHAR(50),
producers_10 VARCHAR(50),	
producers_11 VARCHAR(50),	
producers_12 VARCHAR(50),	
producers_13 VARCHAR(50),	
producers_14 VARCHAR(50),	
producers_15 VARCHAR(50),	
producers_16 VARCHAR(50),	
producers_17 VARCHAR(50),	
producers_18 VARCHAR(50),	
licensors_0	VARCHAR(50),
licensors_1	VARCHAR(50),
licensors_2	VARCHAR(50),
licensors_3	VARCHAR(50),
studios_0 VARCHAR(50),
studios_1 VARCHAR(50),	
studios_2 VARCHAR(50),	
studios_3 VARCHAR(50),	
studios_4 VARCHAR(50),	
studios_5 VARCHAR(50),	
studios_6 VARCHAR(50),	
source VARCHAR(50),
duration VARCHAR(50),	
rating VARCHAR(50),	
rating_description VARCHAR(50),	
ranked INT,
popularity INT,	
members INT,	
favorites INT,	
watching INT,	
completed INT,	
on_hold INT,	
dropped	INT,
plan_to_watch INT,	
score_10 INT,	
score_9 INT,	
score_8 INT,	
score_7 INT,	
score_6 INT,	
score_5 INT,	
score_4 INT,	
score_3 INT,	
score_2 INT,	
score_1 INT);

LOAD DATA INFILE
'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/anime_cleaned.csv'
INTO TABLE anime.anime_cleaned
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 LINES;
```
##### Once the data was imported, I could then begin creating tables. I started with the list of animes titles. 
```sql
CREATE TABLE anime_list 
(SELECT mal_id AS anime_id, title
FROM anime_cleaned);
```

#### For the anime genres, I used UNION and UNION ALL. The
```sql
CREATE TEMPORARY TABLE anime_genre1 
(SELECT mal_id AS anime_id, genres_0 AS genre FROM anime_cleaned WHERE genres_0 IS NOT NULL
UNION ALL
SELECT mal_id AS anime_id, genres_1 FROM anime_cleaned WHERE genres_1 IS NOT NULL
UNION ALL
SELECT mal_id AS anime_id, genres_2 FROM anime_cleaned WHERE genres_2 IS NOT NULL
UNION ALL
SELECT mal_id AS anime_id, genres_3 FROM anime_cleaned WHERE genres_3 IS NOT NULL
UNION ALL
SELECT mal_id AS anime_id, genres_4 FROM anime_cleaned WHERE genres_4 IS NOT NULL
UNION ALL
SELECT mal_id AS anime_id, genres_5 FROM anime_cleaned WHERE genres_5 IS NOT NULL
UNION ALL
SELECT mal_id AS anime_id, genres_6 FROM anime_cleaned WHERE genres_6 IS NOT NULL
UNION ALL
SELECT mal_id AS anime_id, genres_7 FROM anime_cleaned WHERE genres_7 IS NOT NULL
UNION ALL
SELECT mal_id AS anime_id, genres_8 FROM anime_cleaned WHERE genres_8 IS NOT NULL
UNION ALL
SELECT mal_id AS anime_id, genres_9 FROM anime_cleaned WHERE genres_9 IS NOT NULL
UNION ALL
SELECT mal_id AS anime_id, genres_10 FROM anime_cleaned WHERE genres_10 IS NOT NULL
UNION ALL
SELECT mal_id AS anime_id, genres_11 FROM anime_cleaned WHERE genres_11 IS NOT NULL
UNION ALL
SELECT mal_id AS anime_id, genres_12 FROM anime_cleaned WHERE genres_12 IS NOT NULL
ORDER BY anime_id);
```
