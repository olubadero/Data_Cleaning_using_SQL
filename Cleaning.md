### Began the process by creating a database to store the imported data.

```sql 
CREATE DATABASE Hotel_Data;
```

### Actvated the database for use.
```sql
USE Hotel_Data;
```

-- preview table
```sql 
SELECT * FROM `Hotel Feedback`;
```

--  Count number of rows

```sql 
SELECT COUNT(*) FROM `Hotel Feedback`;
```

-- rename the table
```sql 
RENAME TABLE `Hotel Feedback` TO Hotel;
```

-- preview new table name

```sql 
SELECT * FROM `Hotel`;
```

-- Data Cleaning/Exploratory Data Analysis
-- ADD Columns

```sql 
ALTER TABLE Hotel
	ADD COLUMN Gender VARCHAR(10) AFTER `Gender/DOB`,
	ADD COLUMN Date_of_Birth Text AFTER `Gender`, 
      ADD COLUMN Visit_Time Text AFTER `Date_of_Birth`;
```
      
-- Update Table and Split Column

```sql 
Start Transaction;
```

```sql 
Rollback;
```

```sql 
SELECT SUBSTRING_INDEX(`Gender/DOB`, '/', 1) AS Gender, 	
	SUBSTRING_INDEX(SUBSTRING_INDEX(`Gender/DOB`, ' ', 1),'/', -3) AS Date_of_Birth, -- extact from Male/10/2/1993 only then extract the date only
	SUBSTRING_INDEX(`Gender/DOB`, ' ', -2) AS Visit_Time
FROM Hotel;
```


```sql 
UPDATE Hotel
SET Gender = SUBSTRING_INDEX(`Gender/DOB`, '/', 1),	
	Date_of_Birth = SUBSTRING_INDEX(SUBSTRING_INDEX(`Gender/DOB`, ' ', 1),'/', -3),
	Visit_Time = SUBSTRING_INDEX(`Gender/DOB`, ' ', -2);
```
      

-- create date and time column, Split checkout date and update with split value and drop old column 

```sql 
ALTER TABLE hotel
	ADD COLUMN `Checkout_Date` TEXT AFTER `Checkout Date`;
```


```sql 
SELECT SUBSTRING_INDEX(`Checkout Date`, ' ', 1) AS Checkout_Date, 	
FROM Hotel;
```


```sql 
UPDATE Hotel
SET Checkout_Date = SUBSTRING_INDEX(`Checkout Date`, ' ', 1);
```

      
-- Check for blanks
```sql 
SELECT `Gender/DOB`, date_of_birth, Gender,
	SUBSTRING_INDEX(SUBSTRING_INDEX(SUBSTRING_INDEX(`Gender/DOB`, ' ', -3), ' ', 1), '/', -3) AS Fir
FROM hotel
WHERE Date_of_Birth = '';
```
-- There are 3 blank DOB so we will extract and insert the value in the blank spaces

-- code to extract

```sql 
SELECT `Gender/DOB`, date_of_birth, Gender,
	SUBSTRING_INDEX(SUBSTRING_INDEX(SUBSTRING_INDEX(`Gender/DOB`, ' ', -3), ' ', 1), '/', -3) AS Fir
FROM hotel
WHERE Date_of_Birth = '';
```

-- update the column

```sql 
UPDATE Hotel
SET Date_of_Birth = SUBSTRING_INDEX(SUBSTRING_INDEX(SUBSTRING_INDEX(`Gender/DOB`, ' ', -3), ' ', 1), '/', -3)
WHERE Date_of_Birth = '';
```
-- updated 


-- Drop all Unwanted columns

```sql 
ALTER TABLE hotel
	DROP COLUMN `Checkout Date`, 
      DROP COLUMN `Gender/DOB`, 
      DROP COLUMN `visit_time`,
      DROP COLUMN `Checkout_Time`;
```

-- View distinct values in some columns for EDA and cleaning
```sql 
SELECT  DISTINCT gender
FROM hotel;
```
-- there is a need to clean the gender column to ensure consistency

```sql 
SELECT  DISTINCT `guest satisfaction`
FROM hotel;
```
-- between 1-5

```sql 
SELECT  DISTINCT source
FROM hotel;
```
-- trim word of mouth; change organization**; change 'Search***** engine'

```sql 
SELECT  DISTINCT purpose
FROM hotel;
```
-- change oth&er to other; bus to business; vac%ation to vacation to ensure consistency

```sql 
SELECT  DISTINCT feedback
FROM hotel;
```
 -- trim gym and change facility gym to gym to ensure consistency

```sql 
SELECT  DISTINCT category
FROM hotel; -- staffs need to be changed to staff 

```sql 
SELECT  DISTINCT `NPS ratings`
FROM hotel; -- between 3- 10

```sql 
SELECT  DISTINCT `facility ratings`
FROM hotel; 
```

```sql 
SELECT  DISTINCT `Full Name`
FROM hotel;
```

-- Clean out all inconsistent data

```sql 
UPDATE Hotel
SET Gender = TRIM(Gender);
```

```sql 
UPDATE Hotel
SET Gender = 'Male'
WHERE Gender IN ('M', 'Mal');
```

```sql 
UPDATE Hotel
SET Gender = 'Female'
WHERE Gender = 'F'; 
```

```sql 
UPDATE Hotel
SET Source =  TRIM(Source);
```

```sql 
UPDATE Hotel
SET Source =  'Organization'
WHERE Source = 'Organization**';
```

UPDATE Hotel
SET Source =  'Search engine'
WHERE Source = 'Search***** engine';
```

```sql 
UPDATE Hotel
SET Purpose =  'Other'
WHERE Purpose = 'oth&er';
```

```sql 
UPDATE Hotel
SET purpose =  'business'
WHERE purpose = 'bus';
```

```sql 
UPDATE Hotel
SET purpose =  'Vacation'
WHERE purpose = 'Vac&ation';
```

```sql 
UPDATE Hotel
SET feedback =  TRIM(feedback);
```

```sql 
UPDATE Hotel
SET feedback =  'Gym'
WHERE feedback =  'Facility Gym';
```

```sql 
UPDATE Hotel
SET category =  'Staff'
WHERE category =  'Staffs';
```

```sql 
DESCRIBE Hotel;
```

```sql 
SELECT STR_TO_DATE(Date_of_Birth, '%m/%d/%Y'),  STR_TO_DATE(checkout_date, '%m/%d/%Y')
FROM `Hotel`;
```

```sql 
UPDATE Hotel
SET Date_of_Birth = STR_TO_DATE(Date_of_Birth, '%m/%d/%Y'),  
	checkout_date = STR_TO_DATE(checkout_date, '%m/%d/%Y');
```

-- Change data type of date columns

```sql 
ALTER TABLE Hotel
	MODIFY COLUMN Date_of_Birth DATE,
      MODIFY COLUMN Checkout_Date DATE;
```

