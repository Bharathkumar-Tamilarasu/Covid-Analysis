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






