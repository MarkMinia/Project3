# SQL Database Creation: Anime Recommendations 
#### [Original CSV](https://github.com/MarkMinia/Project3/blob/main/Dataset/anime.csv)
#### [Cleaned CSV](https://github.com/MarkMinia/Project3/blob/main/Dataset/anime_cleaned.csv)

#### [Kaggle Link](https://www.kaggle.com/datasets/hernan4444/anime-recommendation-database-2020)

##### Anime shows and movies are an interest of mine and it is what lead me to choosing this dataset. The purpose of this project is to further explore SQL queries and Excel tools by creating my own database.
##### The dataset downloaded from Kaggle contains 17,562 rows, 35 columns, and it was rated a 10 on usability. When going through the CSV, I performed the following actions in Excel:
- ##### Remove duplicates
- ##### Adjust id column series to be in order
- ##### Replace 'Unknown' and blank cells with 'NULL' for text columns
- ##### Replace blank cells with 0 for numeric columns
- ##### Trim spaces and create individual columns for values delimited by commas
- ##### Change the dates to be the same format
- ##### Edit labels in preperation for SQL import

##### Note: Some tables may be too large to view in Github. To view, download the file and open on your desktop. 

##### Before importing the file, I needed to create the table and specify the columns and datatypes. When attempting this through the Import Wizard, there were issues of not capturing all 17,000+ rows, so this step was done manually. The table is structured as such in preperation to normalize the data to 3NF form. It will essentially serve as the base table in which I would query from to create the actual tables that can be used for analysis.    
```sql
CREATE TABLE anime_cleaned 
(anime_id INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
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
producers_19 VARCHAR(50),	
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
##### Once the data was imported, I could then begin creating tables. I started with the list of animes titles. I modified the anime_id column to be the Primary Key for this table; I would use this multiple times in other tables where I need to uniquely identify each row.
```sql
CREATE TABLE anime_list 
(SELECT anime_id, title
FROM anime_cleaned);
ALTER TABLE anime_list
MODIFY anime_id INT PRIMARY KEY AUTO_INCREMENT NOT NULL;
```

##### I continued to work my way through the columns in the order that it was on the CSV. 
```sql
CREATE TABLE average_score 
(SELECT anime_id, score
FROM anime_cleaned);
ALTER TABLE average_score 
MODIFY anime_id INT PRIMARY KEY AUTO_INCREMENT NOT NULL;
```

##### Next, the genres. I used UNION ALL and UNION to create two tables for genres; I would recycle this query for upcoming tables. Most anime shows/movies fit under multiple generes and if I were to select the anime_id and the genres as it is, it would display each id in a row with 13 columns. What I want is two columns where each id and genre is it's own unique row. Basically, the idea behind UNION ALL was to unpivot the table. Even though there appears to be duplicates when looking at the anime_id column, it is still unique when the anime_id and genre are combined, which create composite keys. In the second table, I used UNION to capture each type of genre and added a genre_id to be the Primary Key.
```sql
CREATE TABLE genre_list
(SELECT anime_id, genres_0 AS genre FROM anime_cleaned WHERE genres_0 IS NOT NULL
UNION ALL
SELECT anime_id, genres_1 FROM anime_cleaned WHERE genres_1 IS NOT NULL
UNION ALL
SELECT anime_id, genres_2 FROM anime_cleaned WHERE genres_2 IS NOT NULL
UNION ALL
SELECT anime_id, genres_3 FROM anime_cleaned WHERE genres_3 IS NOT NULL
UNION ALL
SELECT anime_id, genres_4 FROM anime_cleaned WHERE genres_4 IS NOT NULL
UNION ALL
SELECT anime_id, genres_5 FROM anime_cleaned WHERE genres_5 IS NOT NULL
UNION ALL
SELECT anime_id, genres_6 FROM anime_cleaned WHERE genres_6 IS NOT NULL
UNION ALL
SELECT anime_id, genres_7 FROM anime_cleaned WHERE genres_7 IS NOT NULL
UNION ALL
SELECT anime_id, genres_8 FROM anime_cleaned WHERE genres_8 IS NOT NULL
UNION ALL
SELECT anime_id, genres_9 FROM anime_cleaned WHERE genres_9 IS NOT NULL
UNION ALL
SELECT anime_id, genres_10 FROM anime_cleaned WHERE genres_10 IS NOT NULL
UNION ALL
SELECT anime_id, genres_11 FROM anime_cleaned WHERE genres_11 IS NOT NULL
UNION ALL
SELECT anime_id, genres_12 FROM anime_cleaned WHERE genres_12 IS NOT NULL
ORDER BY anime_id);

CREATE TABLE genre_type
(SELECT genres_0 AS genre FROM anime_cleaned WHERE genres_0 IS NOT NULL
UNION
SELECT genres_1 FROM anime_cleaned WHERE genres_1 IS NOT NULL
UNION
SELECT genres_2 FROM anime_cleaned WHERE genres_2 IS NOT NULL
UNION 
SELECT genres_3 FROM anime_cleaned WHERE genres_3 IS NOT NULL
UNION
SELECT genres_4 FROM anime_cleaned WHERE genres_4 IS NOT NULL
UNION
SELECT genres_5 FROM anime_cleaned WHERE genres_5 IS NOT NULL
UNION
SELECT genres_6 FROM anime_cleaned WHERE genres_6 IS NOT NULL
UNION
SELECT genres_7 FROM anime_cleaned WHERE genres_7 IS NOT NULL
UNION
SELECT genres_8 FROM anime_cleaned WHERE genres_8 IS NOT NULL
UNION
SELECT genres_9 FROM anime_cleaned WHERE genres_9 IS NOT NULL
UNION
SELECT genres_10 FROM anime_cleaned WHERE genres_10 IS NOT NULL
UNION
SELECT genres_11 FROM anime_cleaned WHERE genres_11 IS NOT NULL
UNION
SELECT genres_12 FROM anime_cleaned WHERE genres_12 IS NOT NULL);
ALTER TABLE genre_type
ADD genre_id INT PRIMARY KEY AUTO_INCREMENT NOT NULL FIRST
```

##### Here, I created two tables, again. One has been created for each unique media type and another table that displays the anime_id with the corresponding media type. Rather then using Join or Union, I selected the specific columns I needed. I modified anime_id to the primary key for the first table and added media_id to be the primary key for the second table.
```sql
CREATE TABLE media_list 
(SELECT anime_id, media_type
FROM anime_cleaned
ORDER BY anime_id);
ALTER TABLE media_list
MODIFY anime_id INT PRIMARY KEY AUTO_INCREMENT NOT NULL;

CREATE TABLE media_type 
(SELECT media_type
FROM anime_cleaned
WHERE media_type IS NOT NULL
GROUP BY media_type);
ALTER TABLE media_type
ADD media_id INT PRIMARY KEY AUTO_INCREMENT NOT NULL FIRST;
```

##### I applied the same concept here as the media tables.
```sql
CREATE TABLE anime_airing 
(SELECT anime_id, episodes, aired, premiered 
FROM anime_cleaned);
ALTER TABLE anime_airing
MODIFY anime_id INT PRIMARY KEY AUTO_INCREMENT NOT NULL;
```

##### I took the same concept from earlier when dealing with the generes and applied it to the list of producers. 
```sql
CREATE TABLE producer_list
(SELECT anime_id, producers_0 as producer FROM anime_cleaned WHERE  producers_0 IS NOT NULL
UNION ALL
SELECT anime_id, producers_1 FROM anime_cleaned WHERE  producers_1 IS NOT NULL
UNION ALL
SELECT anime_id, producers_2 FROM anime_cleaned WHERE  producers_2 IS NOT NULL
UNION ALL
SELECT anime_id, producers_3 FROM anime_cleaned WHERE  producers_3 IS NOT NULL
UNION ALL
SELECT anime_id, producers_4 FROM anime_cleaned WHERE  producers_4 IS NOT NULL
UNION ALL
SELECT anime_id, producers_5 FROM anime_cleaned WHERE producers_5 IS NOT NULL
UNION ALL
SELECT anime_id, producers_6 FROM anime_cleaned WHERE producers_6 IS NOT NULL
UNION ALL
SELECT anime_id, producers_7 FROM anime_cleaned WHERE producers_7 IS NOT NULL
UNION ALL
SELECT anime_id, producers_8 FROM anime_cleaned WHERE producers_8 IS NOT NULL
UNION ALL
SELECT anime_id, producers_9 FROM anime_cleaned WHERE producers_9 IS NOT NULL
UNION ALL
SELECT anime_id, producers_10 FROM anime_cleaned WHERE producers_10 IS NOT NULL
UNION ALL
SELECT anime_id, producers_11 FROM anime_cleaned WHERE producers_11 IS NOT NULL
UNION ALL
SELECT anime_id, producers_12 FROM anime_cleaned WHERE producers_12 IS NOT NULL
UNION ALL
SELECT anime_id, producers_13 FROM anime_cleaned WHERE producers_13 IS NOT NULL
UNION ALL
SELECT anime_id, producers_14 FROM anime_cleaned WHERE producers_14 IS NOT NULL
UNION ALL
SELECT anime_id, producers_15 FROM anime_cleaned WHERE producers_15 IS NOT NULL
UNION ALL
SELECT anime_id, producers_16 FROM anime_cleaned WHERE producers_16 IS NOT NULL
UNION ALL
SELECT anime_id, producers_17 FROM anime_cleaned WHERE producers_17 IS NOT NULL
UNION ALL
SELECT anime_id, producers_18 FROM anime_cleaned WHERE producers_18 IS NOT NULL
UNION ALL
SELECT anime_id, producers_19 FROM anime_cleaned WHERE producers_18 IS NOT NULL
ORDER BY anime_id);

CREATE TABLE producers
(SELECT producers_0 as producer FROM anime_cleaned WHERE producers_0 IS NOT NULL
UNION 
SELECT producers_1 FROM anime_cleaned WHERE producers_1 IS NOT NULL
UNION 
SELECT producers_2 FROM anime_cleaned WHERE producers_2 IS NOT NULL
UNION 
SELECT producers_3 FROM anime_cleaned WHERE  producers_3 IS NOT NULL
UNION 
SELECT producers_4 FROM anime_cleaned WHERE producers_4 IS NOT NULL
UNION 
SELECT producers_5 FROM anime_cleaned WHERE producers_5 IS NOT NULL
UNION 
SELECT producers_6 FROM anime_cleaned WHERE producers_6 IS NOT NULL
UNION 
SELECT producers_7 FROM anime_cleaned WHERE producers_7 IS NOT NULL
UNION 
SELECT producers_8 FROM anime_cleaned WHERE producers_8 IS NOT NULL
UNION 
SELECT producers_9 FROM anime_cleaned WHERE producers_9 IS NOT NULL
UNION 
SELECT producers_10 FROM anime_cleaned WHERE producers_10 IS NOT NULL
UNION ALL
SELECT producers_11 FROM anime_cleaned WHERE producers_11 IS NOT NULL
UNION 
SELECT producers_12 FROM anime_cleaned WHERE producers_12 IS NOT NULL
UNION 
SELECT producers_13 FROM anime_cleaned WHERE producers_13 IS NOT NULL
UNION 
SELECT producers_14 FROM anime_cleaned WHERE producers_14 IS NOT NULL
UNION 
SELECT producers_15 FROM anime_cleaned WHERE producers_15 IS NOT NULL
UNION 
SELECT producers_16 FROM anime_cleaned WHERE producers_16 IS NOT NULL
UNION 
SELECT producers_17 FROM anime_cleaned WHERE producers_17 IS NOT NULL
UNION 
SELECT producers_18 FROM anime_cleaned WHERE producers_18 IS NOT NULL
UNION 
SELECT producers_19 FROM anime_cleaned WHERE producers_19 IS NOT NULL);
ALTER TABLE producers
ADD producer_id INT PRIMARY KEY AUTO_INCREMENT NOT NULL FIRST
```

##### I took the same concept from earlier when dealing with the generes and applied it to the list of licensors.
```sql
CREATE TABLE licensor_list
(SELECT anime_id, licensors_0 AS licensor FROM anime_cleaned WHERE licensors_0 IS NOT NULL
UNION ALL
SELECT anime_id, licensors_1 FROM anime_cleaned WHERE licensors_1 IS NOT NULL
UNION ALL
SELECT anime_id, licensors_2 FROM anime_cleaned WHERE licensors_2 IS NOT NULL
UNION ALL
SELECT anime_id, licensors_3 FROM anime_cleaned WHERE licensors_3 IS NOT NULL
ORDER BY anime_id);

CREATE TABLE licensors 
(SELECT licensors_0 AS licensor FROM anime_cleaned WHERE licensors_0 IS NOT NULL
UNION
SELECT licensors_1 FROM anime_cleaned WHERE licensors_1 IS NOT NULL
UNION
SELECT licensors_2 FROM anime_cleaned WHERE licensors_2 IS NOT NULL
UNION 
SELECT licensors_3 FROM anime_cleaned WHERE licensors_3 IS NOT NULL);
ALTER TABLE licensors
ADD licensor_id INT PRIMARY KEY AUTO_INCREMENT NOT NULL FIRST;
```

##### I took the same concept from earlier when dealing with the generes and applied it to the list of studios.
```sql
CREATE TABLE studio_list 
(SELECT anime_id, studios_0 AS studios FROM anime_cleaned WHERE studios_0 IS NOT NULL
UNION ALL
SELECT anime_id, studios_1 FROM anime_cleaned WHERE studios_1 IS NOT NULL
UNION ALL
SELECT anime_id, studios_2 FROM anime_cleaned WHERE studios_2 IS NOT NULL
UNION ALL
SELECT anime_id, studios_3 FROM anime_cleaned WHERE studios_3 IS NOT NULL
UNION ALL
SELECT anime_id, studios_4 FROM anime_cleaned WHERE studios_4 IS NOT NULL
UNION ALL
SELECT anime_id, studios_5 FROM anime_cleaned WHERE studios_5 IS NOT NULL
UNION ALL
SELECT anime_id, studios_6 FROM anime_cleaned WHERE studios_6 IS NOT NULL
ORDER BY anime_id);

CREATE TABLE studios 
(SELECT studios_0 AS studios FROM anime_cleaned WHERE studios_0 IS NOT NULL
UNION
SELECT studios_1 FROM anime_cleaned WHERE studios_1 IS NOT NULL
UNION
SELECT studios_2 FROM anime_cleaned WHERE studios_2 IS NOT NULL
UNION 
SELECT studios_3 FROM anime_cleaned WHERE studios_3 IS NOT NULL
UNION
SELECT studios_4 FROM anime_cleaned WHERE studios_4 IS NOT NULL
UNION
SELECT studios_5 FROM anime_cleaned WHERE studios_5 IS NOT NULL
UNION
SELECT studios_6 FROM anime_cleaned WHERE studios_6 IS NOT NULL);
ALTER TABLE studios
ADD studio_id INT PRIMARY KEY AUTO_INCREMENT NOT NULL FIRST;
```

##### Like the media tables, I applied the same concept for all remaining tables below:
```sql
CREATE TABLE source_list
(SELECT  anime_id, source
FROM anime_cleaned
ORDER BY anime_id);

CREATE TABLE source_type
(SELECT source
FROM anime_cleaned
WHERE source IS NOT NULL
GROUP BY source);
ALTER TABLE source_type
ADD source_id INT PRIMARY KEY AUTO_INCREMENT NOT NULL FIRST;
```

#####
```sql
CREATE TABLE duration 
(SELECT anime_id, duration
FROM anime_cleaned);
ALTER TABLE duration
MODIFY anime_id INT PRIMARY KEY AUTO_INCREMENT NOT NULL;
```

#####
```sql
CREATE TABLE rating_list
(SELECT anime_id, rating, rating_description
FROM anime_cleaned
ORDER BY anime_id);

CREATE TABLE rating_type
(SELECT rating, rating_description
FROM anime_cleaned
WHERE rating IS NOT NULL
GROUP BY rating);
ALTER TABLE rating_type
ADD rating_id INT PRIMARY KEY AUTO_INCREMENT NOT NULL FIRST;
```

#####
```sql
CREATE TABLE viewership 
(SELECT anime_id, ranked, popularity, members, favorites,
watching, completed, on_hold, dropped, plan_to_watch
FROM anime_cleaned);
ALTER TABLE viewership
MODIFY anime_id INT PRIMARY KEY AUTO_INCREMENT NOT NULL;
```

#####
```sql
CREATE TABLE scoring
(SELECT anime_id, score_10, score_9, score_8,
score_7, score_6, score_5, score_4, score_3, score_2, score_1
FROM anime_cleaned);
ALTER TABLE scoring
MODIFY anime_id INT PRIMARY KEY AUTO_INCREMENT NOT NULL;
```
