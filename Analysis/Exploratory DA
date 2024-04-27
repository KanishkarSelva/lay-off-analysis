-------------------------------------------------------------------------------------------
-- Exploratory Data Analysis

select * from
layoffs_staging2;

select max(total_laid_off), max(percentage_laid_off)
from layoffs_staging2;

select * from
layoffs_staging2
where percentage_laid_off= 1
order by funds_raised_millions desc

-- total layoff by company
select company, sum(total_laid_off)
from
layoffs_staging2
group by company
order by 2 desc;

-- viewing the date
select min(date), max(date)
from
layoffs_staging2;

-- total layoff by industry
select industry, sum(total_laid_off)
from
layoffs_staging2
group by industry
order by 2 desc;

select *
from
layoffs_staging2;

-- total layoff by industry
select country, sum(total_laid_off)
from
layoffs_staging2
group by country
order by 2 desc;

-- total layoff by date
select year(date), sum(total_laid_off)
from
layoffs_staging2
group by year(date)
order by 1 desc;

-- total layoff by stage
select stage, sum(total_laid_off)
from
layoffs_staging2
group by stage
order by 2 desc;

-- avg percentage layoff by company
select company, avg(percentage_laid_off)
from
layoffs_staging2
group by company
order by 2 desc;

-- layoff in month wise and year wise
select substring(date, 1,7) as month, sum(total_laid_off)
from layoffs_staging2
where substring(date, 1,7) is not null
group by month 
order by 1 asc;

-- rolling total
with Rolling_Total as
(
select substring(date, 1,7) as month, sum(total_laid_off) as total_off
from layoffs_staging2
where substring(date, 1,7) is not null
group by month 
order by 1 asc
)

select month, total_off,
sum(total_off) over(order by month) as rolling_total
from Rolling_Total;


select company, sum(total_laid_off)
from
layoffs_staging2
group by company
order by 2 desc;

select company,year(date),sum(total_laid_off)
from
layoffs_staging2
group by company, date
order by 3 desc;

with Company_Year (company, years, total_laidoff) as
(
select company,year(date),sum(total_laid_off)
from
layoffs_staging2
group by company, year(date)
), 
Company_year_rank as
(
select *, dense_rank() over (partition by years order by total_laidoff desc)
as Ranking
from Company_Year
where years is not null
)
select *
from Company_year_rank
where Ranking <= 5;








































