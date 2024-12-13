# Netflix Movies and TV Shows Data Analysis using MS SQL Server
![Netflix Logo](https://github.com/jananiravi310704/Netflix_MS-SQL_Project/blob/main/netflix.logo.png)
DESCRIPTION:
     The Netflix SQL Server Project is a comprehensive database system designed to simulate the backend operations of a streaming platform like Netflix. This project demonstrates the use of SQL Server to model, store, and manage data related to users, content, subscriptions, and analytics. It provides a foundation for building data-driven applications, performing advanced analytics, and supporting scalable user services.

The QUERY and SOLUTION, Where shown below,

--1.COUNT THE MOVIES AND TV SHOW 
SELECT 
   type,
   count(*) as total_count
   FROM [dbo].['netflix dataset$']
   GROUP BY type

   --2.FIND THE MOST  COMMON RATING FOR MOVIES AND TV SHOWS
	SELECT 
    type, 
    rating, 
    RatingCount
FROM (
    SELECT 
        type, 
        rating, 
        COUNT(*) AS RatingCount,
        RANK() OVER (PARTITION BY Type ORDER BY COUNT(*) DESC) AS Rank
    FROM 
        dbo.['netflix dataset$']
    GROUP BY 
        type, rating
) RankedRatings
WHERE 
    Rank = 1;

--3.list all movies released in specific year 2020
SELECT * 
FROM dbo.['netflix dataset$']
WHERE
   type='movie'
AND
   release_year ='2020'

--4.identify the longest movie 
SELECT * FROM dbo.['netflix dataset$']
WHERE
    type='Movie'
    AND 
    duration=(SELECT MAX(duration) from dbo.['netflix dataset$'])

--5.find the content added in last five year

   SELECT *
FROM dbo.['netflix dataset$']
WHERE date_added >= DATEADD(YEAR, -5, GETDATE());

--6.find the all tv shows and movie show directed by 'Rajiv Chilaka'

   SELECT *
FROM dbo.['netflix dataset$']
WHERE 
   director LIKE'%Rajiv Chilaka%'

--7.list the tv show with more than 5 seasons
SELECT * 
FROM dbo.['netflix dataset$']
WHERE
   type='TV Show'
AND
   duration>= '5 Seasons'

--8.list all the movie in documentaries
SELECT 
    type,
	listed_in
FROM dbo.['netflix dataset$']
WHERE
   listed_in LIKE'%Documentaries%'

--9.FIND ALL CONTENT WITHOUL DIRECTOR
 SELECT 
      director
FROM dbo.['netflix dataset$']
WHERE
   director IS NULL

 --10.HOW MANY MOVIE ACTOR 'SALMAN KHAN' appeared in last 10 year
 SELECT 
      type='Movie',
	  release_year,
	  cast
FROM dbo.['netflix dataset$']
WHERE
    cast LIKE'%Salman Khan%'
AND
    release_year BETWEEN 2014 AND 2024;

--11. FIND THE TOP 5 COUNTRIES IN THE MOST COMMON ON NETFLIX
 SELECT TOP 5 
    country, 
    COUNT(*) AS title_count
FROM 
    dbo.['netflix dataset$']
GROUP BY 
    country
ORDER BY 
    title_count DESC;

--12.Find duplicate titles in the database.
SELECT title,COUNT(*) as duplicate_title   
FROM DBO.['netflix dataset$']
GROUP BY title
Having count(*)>1;

--13.Calculate the average duration of movies.
 SELECT 
    AVG(CAST(LEFT(duration, CHARINDEX(' ', duration + ' ') - 1) AS INT)) AS avg_duration
FROM 
    dbo.['netflix dataset$']
WHERE 
    type = 'Movie';

--14.Find all content with "Love" in the title.
 SELECT *
 FROM dbo.['netflix dataset$'] 
 WHERE title LIKE '%Love%';

 --15.Find countries that have multiple content items listed (more than 10).
 SELECT country, COUNT(*) AS total_count 
FROM dbo.['netflix dataset$']
GROUP BY country 
HAVING COUNT(*) > 10;

--16.spit the country from string to array
SELECT value AS country 
FROM dbo.['netflix dataset$']
CROSS APPLY STRING_SPLIT(country, ',');

--17.count the country and their values
SELECT 
    value AS country, 
    COUNT(*) AS country_count
FROM 
     dbo.['netflix dataset$']
CROSS APPLY 
    STRING_SPLIT(country, ',')
GROUP BY 
    value
ORDER BY 
    country_count DESC;

