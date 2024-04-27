----------------------------------------------------------------------------
-- Data Cleaning
select *
from layoffs

-- 1. Removing the duplicates
-- 2. Standardize the data ( Any Issues with data like spelling mistakes or other issues)
-- 3. No Values and Blank Values
-- 4. Remove the Irrelevant Columns to optimize the query

-- Creating the staging table to make the original raw table intact.
Create table layoffs_staging
like layoffs

-- Viewing the table before the data inserted
select *
from layoffs_staging

-- Inserting the rows or data in the layoffs_staging
insert layoffs_staging
select *
from layoffs

-- Viewing the table after the data inserted
select *
from layoffs_staging

-- 1. Removing the duplicates
-- Finding the duplicates with row number

select *, row_number() over(
partition by company, location, industry, total_laid_off, percentage_laid_off, `date`, country, funds_raised_millions ) as row_num
from layoffs_staging

-- creating cte to identify the duplicates

with duplicate as
(
select *, row_number() over(
partition by company, location, industry, total_laid_off, percentage_laid_off, `date`, country, funds_raised_millions ) as row_num
from layoffs_staging
)
select *
from duplicate
where row_num > 1;

-- Deleting the duplicates (Plan 1)
-- (which is not worked - Error - Error Code: 1288. The target table duplicate of the DELETE is not updatable)
with duplicate as
(
select *, row_number() over(
partition by company, location, industry, total_laid_off, percentage_laid_off, `date`, country, funds_raised_millions ) as row_num
from layoffs_staging
)
delete
from duplicate
where row_num > 1;

-- Deleting the duplicates (Plan 2)
-- Creating the 2nd staging table (Copied tfrom clipboard of the table)
CREATE TABLE `layoffs_staging2` (
  `company` text,
  `location` text,
  `industry` text,
  `total_laid_off` int DEFAULT NULL,
  `percentage_laid_off` text,
  `date` text,
  `stage` text,
  `country` text,
  `funds_raised_millions` int DEFAULT NULL,
  `row_num` INT
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;

-- Viweing the 2nd staging table before the data inserted
select *
from layoffs_staging2;

-- inserting the data from staging 1 to staging 2 
insert into layoffs_staging2
select *, row_number() over(
partition by company, location, industry, total_laid_off, percentage_laid_off, `date`, country, funds_raised_millions ) as row_num
from layoffs_staging;

-- Viweing the 2nd staging table after the data inserted
select *
from layoffs_staging2;

-- Filtering the row_num
select *
from layoffs_staging2
where row_num > 1;

-- Deleting the duplicates
delete
from layoffs_staging2
where row_num > 1;

-- Faced error (Error Code: 1175. You are using safe update mode and you tried to update a table without a WHERE that uses a KEY column.  To disable safe mode, toggle the option in Preferences -> SQL Editor and reconnect.)
SET SQL_SAFE_UPDATES = 0;

-- Duplicates deleted!!

-- Now STEP 2 -- Standardize the data ( Any Issues with data like spelling mistakes or other issues)
-- Viewing the white spaces of begin and end
select distinct(company), trim(company)
from 
layoffs_staging2;

-- Removing the white spaces of begin and end
update layoffs_staging2
set company = trim(company);

-- viewing the repetitive value or spelling mistakes
select distinct industry
from 
layoffs_staging2
order by industry;

-- finding the values of the mistakes
select *
from 
layoffs_staging2
where industry like "Crypto%";

-- Rectifying the values 
update layoffs_staging2
set industry = "Crypto"
where industry like "Crypto%"

-- viewing the correction we done
select distinct industry
from 
layoffs_staging2
order by industry;

-- viewing any other mistakes or errors
select distinct country
from layoffs_staging2
order by 1;

-- viewing any other mistakes or errors
select *
from layoffs_staging2
where country like "United States."

-- correcting this with Trim Trailing method. Trailing used to remove the particular character from end.
select distinct country, trim(trailing '.' from country)
from layoffs_staging2
order by 1;

-- correcting the errors
update layoffs_staging2 
set country = trim(trailing '.' from country)
where country like "United States%"

-- viewing it after correcting the errors
select distinct country
from layoffs_staging2
order by 1;

-- converting the date text format to datetime
-- viewing the date column
select date
from layoffs_staging2

-- viewing the date column
select date,
str_to_date(date, '%m/%d/%Y')
from layoffs_staging2

-- update this date "text" to date "date"
update layoffs_staging2
set date = str_to_date(date, '%m/%d/%Y')

-- viewing the date column after updating
select date
from layoffs_staging2
order by 1;

-- Changing the date format
alter table layoffs_staging2
modify column `date` date;

-- 3. Null and Black values
-- looking into nulls
select *
from layoffs_staging2
where total_laid_off is null;

select *
from layoffs_staging2
where total_laid_off is null and
percentage_laid_off is null;

select distinct industry
from layoffs_staging2
where industry is null or industry = "";

select distinct industry
from layoffs_staging2
where company = "Airbnb";

-- viewing the columns to popoulate the blank statement to values data 
select *
from layoffs_staging2 t1
join layoffs_staging2 t2
	on t1.company = t2.company and t1.location = t2.location
where (t1.industry is null or t1.industry = "") and t2.industry is not null
;

-- updating the column (not worked)
update layoffs_staging2 t1
join layoffs_staging2 t2
	on t1.company = t2.company
set t1.industry = t2.industry
where t1.industry is null 
and t2.industry is not null;

-- Trying to make the black to null then will set the values accordingly
update layoffs_staging2
set industry = NULL
where industry = "";

-- updating the column (now, it worked)
update layoffs_staging2 t1
join layoffs_staging2 t2
	on t1.company = t2.company
set t1.industry = t2.industry
where t1.industry is null 
and t2.industry is not null;

-- viewing any other row had null
select *
from layoffs_staging2
where company like "Bally%";

-- Removing the columns where we don't have any values which is not going to help us
select *
from layoffs_staging2
where total_laid_off is null and
percentage_laid_off is null;

-- Removing the columns where we don't have any values which is not going to help us
DELETE
from layoffs_staging2
where total_laid_off is null and
percentage_laid_off is null;

select *
from layoffs_staging2;

-- dropping the row number
alter table layoffs_staging2
drop column row_num;

------------------------------------------------------------------------------------------------
































