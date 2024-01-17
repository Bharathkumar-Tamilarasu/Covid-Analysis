# Covid-19 : An Analytical Study

## Question and Answers

**Author**: Bharathkumar Tamilarasu <br />
**Email**: bharathkumar.t.17@gmail.com <br />
**Website**: https://bharathkumart17.wixsite.com/portfolio <br />
**LinkedIn**: https://www.linkedin.com/in/bharathkumar-tamilarasu-218429222/  <br />
##
### Breaking things down by Country

**1.**  What is the liklihood of dying if you contract covid (in India)?

````sql
SELECT LOCATION,DATE,TOTAL_CASES,
	TOTAL_DEATHS,
	(TOTAL_DEATHS * 1.0 / TOTAL_CASES * 1.0) * 100 AS DEATH_PERCENTAGE
FROM COVID_DEATHS
WHERE LOCATION = 'India'
	AND CONTINENT IS NOT NULL
ORDER BY 1,2 desc
````

**Results: Latest 5 values**



**2.**  What is the percentage of population infected with covid (in India)?

````sql
SELECT LOCATION,DATE,POPULATION,
	TOTAL_CASES,
	(TOTAL_CASES * 1.0 / POPULATION * 1.0) * 100 AS INFECTED_PERCENTAGE
FROM COVID_DEATHS
WHERE LOCATION = 'India'
	AND CONTINENT IS NOT NULL
ORDER BY 1,2 desc
````

**Results: Latest 5 values**



**3.**  What are the top 10 countries with the highest infection rates compared to their populations?

````sql
SELECT LOCATION,
	POPULATION,
	MAX(TOTAL_CASES) AS HIGHEST_INFECTION_COUNT,
	MAX((TOTAL_CASES * 1.0 / POPULATION * 1.0) * 100) AS HIGHEST_INFECTED_PERCENTAGE
FROM COVID_DEATHS
WHERE CONTINENT IS NOT NULL
	AND TOTAL_DEATHS IS NOT NULL GROUP
	BY 1,2
ORDER BY 4 DESC
````

**Results: Latest 5 values**



**4.**  What are the top 10 countries with the highest death count?

````sql
SELECT LOCATION,
	MAX(TOTAL_DEATHS) AS HIGHEST_DEATH_COUNT
FROM COVID_DEATHS
WHERE CONTINENT IS NOT NULL
	AND TOTAL_DEATHS IS NOT NULL
GROUP BY 1
ORDER BY 2 DESC
````

**Results: Latest 5 values**


### Breaking things down by Continent

**5.**  What is the total death count per continent?

````sql
SELECT CONTINENT,
	SUM(TOTAL_DEATHS) AS TOTAL_DEATHS
FROM COVID_DEATHS
WHERE CONTINENT IS NOT NULL
GROUP BY 1
ORDER BY 2 DESC
````

**Results: Latest 5 values**

### Global numbers

**6.**  What is the global total death count?

````sql
SELECT SUM(NEW_CASES) AS TOTAL_CASES,
	SUM(NEW_DEATHS) AS NEW_DEATHS,
	(SUM(NEW_DEATHS) * 1.0 / SUM(NEW_CASES) * 1.0) * 100 AS DEATH_PERCENTAGE
FROM COVID_DEATHS
WHERE CONTINENT IS NOT NULL
````

**Results: Latest 5 values**

**7.**  What is the number of people who have received at least one COVID vaccine in India?

````sql
SELECT DE.LOCATION,
	DE.DATE,
	DE.POPULATION,
	VA.NEW_VACCINATIONS AS DAILY_NEW_VACCINATIONS,
	SUM(VA.NEW_VACCINATIONS) OVER(PARTITION BY DE.LOCATION
		ORDER BY DE.LOCATION ASC,DE.DATE ASC) AS TOTAL_VACCINATIONS
FROM COVID_DEATHS DE
JOIN COVID_VACCINATIONS VA ON DE.LOCATION = VA.LOCATION
AND DE.DATE = VA.DATE
WHERE DE.CONTINENT IS NOT NULL
	AND DE.LOCATION = 'India'
ORDER BY 2 DESC
````

**Results: Latest 5 values**


**8.**  What is the percentage of the population that has received at least one COVID vaccine?

````sql
WITH POP_VAC_CTE (LOCATION,DATE,POPULATION, NEW_VACCINATIONS, TOTAL_VACCINATIONS) 
AS
	(
	SELECT DE.LOCATION,
		DE.DATE,
		DE.POPULATION,
		VA.NEW_VACCINATIONS AS DAILY_NEW_VACCINATIONS,
		SUM(VA.NEW_VACCINATIONS) OVER(PARTITION BY DE.LOCATION
			ORDER BY DE.LOCATION ASC,DE.DATE ASC) AS TOTAL_VACCINATIONS
	FROM COVID_DEATHS DE
	JOIN COVID_VACCINATIONS VA ON DE.LOCATION = VA.LOCATION
	AND DE.DATE = VA.DATE
	WHERE DE.CONTINENT IS NOT NULL
	)
		
SELECT *,(TOTAL_VACCINATIONS * 1.0 / POPULATION * 1.0) * 100 AS TOTAL_VACCINATIONS_PERCENTAGE
FROM POP_VAC_CTE
WHERE LOCATION = 'India'
ORDER BY 1,2 DESC
````

**Results: Latest 5 values**


