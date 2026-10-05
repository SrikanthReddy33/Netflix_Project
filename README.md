# Netflix Movies and TV Shows Data Analysis using SQL


## Overview
This project involves a comprehensive analysis of Netflix's movies and TV shows data using SQL. The goal is to extract valuable insights and answer various business questions based on the dataset. The following README provides a detailed account of the project's objectives, business problems, solutions, findings, and conclusions.

## Objectives

- Analyze the distribution of content types (movies vs TV shows).
- Identify the most common ratings for movies and TV shows.
- List and analyze content based on release years, countries, and durations.
- Explore and categorize content based on specific criteria and keywords.

## Dataset

The data for this project is sourced from the Kaggle dataset:

- **Dataset Link:** [Movies Dataset](https://github.com/SrikanthReddy33/Netflix_Project/blob/main/netflix_titles.csv)

## Schema



##-- Netflix Project
drop table if exists Netflix;
create table Netflix
(
    show_id	     varchar(15),
	type	     varchar(20),
	title	     varchar(120),
	director     varchar(220),
	casts	     varchar(800),
	country	     varchar(130),
	date_added   varchar(50),
	release_year int,
	rating	     varchar(10),
	duration	 varchar(15),
	listed_in	 varchar(100),
	description  varchar(250)

)


select * from Netflix;



## Business Problems and Solutions


##-- 1. Count the number of Movies vs TV Shows.

select 
	type,
	count(*) 
from Netflix
group by type;



-- 2. Find the most common rating for movies and TV shows.


select type,
	   rating,
	   count(*)
from Netflix
group by 1,2
order by count desc



-- 3. List all movies released in a specific year (e.g., 2020).


select title,
	   release_year
from Netflix
where release_year = '2020';


-- 4. Find the top 5 countries with the most content on Netflix.

select unnest(string_to_array(country, ',')) as new_country,
		count(show_id) as total_count
from Netflix
group by 1
order by 2 desc
limit 5;


-- select 
-- 		unnest(string_to_array(country, ',')) as new_country
-- from Netflix;



-- 5. Identify the longest movie


select * 
from Netflix
where type ='Movie'
	  and
	  duration = (select max(duration)from Netflix);




-- 6. Find content added in the last 5 years


select 
		*
from Netflix
where  TO_DATE(date_added,'Month DD,YYYY')>= current_date - interval '5 Years'




-- 7. Find all the movies/TV shows by director 'Rajiv Chilaka'!

--1

select * 
from Netflix
where director like '%Rajiv Chilaka%';

--2 

select * 
from (
	   select *,
	   unnest(string_to_array(director,',')) as director_name
	   from Netflix)
where director = 'Rajiv Chilaka'


-- 8. List all TV shows with more than 5 seasons.


select * 
from Netflix
where type = 'TV Show' and duration > '5 Season';


-- 9. Count the number of content items in each genre.


select 
	unnest(string_to_array(listed_in, ',')) as content,
	count(*) as count
from Netflix
group by content
order by count desc;



-- 10. Find each year and the average numbers of content release by India on netflix. 
-- return top 5 year with highest avg content release !


select
		extract(Year from to_date(date_added,'Month,dd,YYYY')) as year,
		count(*)
from Netflix
where country = 'India'
group by 1
order by 1
limit 5;


-- 11. List all movies that are documentaries.


select * 
from Netflix
where listed_in like '%Documentaries%';


-- 12. Find all content without a director.

select * 
from Netflix
where director is null;




-- 13. Find how many movies actor 'Salman Khan' appeared in last 10 years!


select
		count(*)
from Netflix
where casts like '%Salman Khan%';


-- 14. Find the top 10 actors who have appeared in the highest number of movies produced in India.

select unnest(string_to_array(casts,','))as actors,
		count(*)
from Netflix
where country = 'India'
group by actors
order by 2 desc
limit 10;




## Findings and Conclusion

- **Content Distribution:** The dataset contains a diverse range of movies and TV shows with varying ratings and genres.
- **Common Ratings:** Insights into the most common ratings provide an understanding of the content's target audience.
- **Geographical Insights:** The top countries and the average content releases by India highlight regional content distribution.
- **Content Categorization:** Categorizing content based on specific keywords helps in understanding the nature of content available on Netflix.

This analysis provides a comprehensive view of Netflix's content and can help inform content strategy and decision-making.

