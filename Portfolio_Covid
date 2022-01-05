--selecting data which I will be using 
SELECT location, date, total_cases, new_cases, total_deaths, population
FROM `capstone-profject.Covid_deaths.covid_deaths` 
WHERE continent is not null 
Order by 1,2


-- Looking at Total cases vs. Total deaths
-- Shows chances of dying if you contract covid in your country
SELECT location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 AS death_percenatge
FROM `capstone-profject.Covid_deaths.covid_deaths` 
WHERE location like '%States%' AND continent IS NOT NULL 
Order by 1,2


--Looking at Total Cases Vs. Population
-- Shows percentage of population with covid
SELECT location, date, population, total_cases, (total_cases/population)*100 AS Percentpopulation_infected
FROM `capstone-profject.Covid_deaths.covid_deaths` 
WHERE location like '%States%' AND continent IS NOT NULL 
Order by 1,2

-- Looking at Total Deaths Vs. Population
-- Shows percentage of Covid deaths
SELECT location, date, population, total_deaths, (total_deaths/population)*100 AS death_total
FROM `capstone-profject.Covid_deaths.covid_deaths` 
WHERE location like '%States%' AND continent IS NOT NULL 
Order by 1,2

-- Looking at countries with the highest infection rates compared to population
SELECT location, population, MAX(total_cases) AS HighestInfectionCount, MAX((total_cases/population))*100 AS totalPercentpopulation_infected
FROM `capstone-profject.Covid_deaths.covid_deaths` 
WHERE continent IS NOT NULL 
Group by location, population
Order by totalPercentpopulation_infected DESC

-- Showing countries with Highest Death Count per population
SELECT location, MAX(total_deaths) AS TotalDeathCount
FROM `capstone-profject.Covid_deaths.covid_deaths` 
WHERE continent IS NOT NULL 
Group by location
Order by TotalDeathCount DESC

-- BREAKING THINGS DOWN BY CONTINENT

-- Showing Continents with the highest death count per population
SELECT continent, MAX(total_deaths) AS ContinentDeathCount
FROM `capstone-profject.Covid_deaths.covid_deaths` 
WHERE continent IS NOT NULL 
Group by continent
Order by ContinentDeathCount DESC

-- GLOBAL NUMBERS

SELECT SUM(new_cases) AS total_cases, SUM(new_deaths) AS total_deaths, SUM(new_deaths)/SUM(new_cases)*100 AS Global_death_percentage
FROM `capstone-profject.Covid_deaths.covid_deaths` 
WHERE continent IS NOT NULL 
Order by 1,2


-- Joining Deaths Table with Vaccinations Table
SELECT *
FROM `capstone-profject.Covid_deaths.covid_deaths` AS dea
JOIN `capstone-profject.Covid_deaths.covid_vaccinations` AS vac
    ON dea.location = vac.location
    AND dea.date = vac.date

-- Looking at New Vaccinations AS Rolling Count
SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations, 
        SUM(vac.new_vaccinations) OVER (PARTITION BY dea.location ORDER BY dea.location, dea.date) AS Rolling_people_vaccinated,
FROM `capstone-profject.Covid_deaths.covid_deaths` AS dea
JOIN `capstone-profject.Covid_deaths.covid_vaccinations` AS vac
    ON dea.location = vac.location
    AND dea.date = vac.date
WHERE dea.continent IS NOT NULL
Order by 2,3

-- Looking at Total Population Vs Total Vaccinated
SELECT dea.continent, dea.location, (MAX((vac.people_vaccinated)/dea.population))*100 AS vaccinated_population
FROM `capstone-profject.Covid_deaths.covid_deaths` AS dea
JOIN `capstone-profject.Covid_deaths.covid_vaccinations` AS vac
    ON dea.location = vac.location
WHERE dea.continent IS NOT NULL
GROUP BY dea.continent, dea.location
Order by vaccinated_population DESC
