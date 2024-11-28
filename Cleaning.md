## `Data Importation Process:`

#### Began the process by creating a database to store the imported CSV data.

```sql 
CREATE DATABASE Hotel_Data;
```

#### Activated the database for use.
```sql
USE Hotel_Data;
```

#### Previewed the table and counted the rows to ensure all data was imported correctly.
```sql 
SELECT *
FROM `Hotel Feedback`;
```

![image](https://github.com/user-attachments/assets/0295e69b-2c74-42f4-9f71-1142ae951e10)


```sql 
SELECT COUNT(*) FROM `Hotel Feedback`;
```
#### All 15, 583 rows and 11 columns were correctly imported.


#### Renamed the table.

```sql 
RENAME TABLE `Hotel Feedback` TO Hotel;
```

## `Exploratory Data Analysis and Data Cleaning Process:`

#### Added new Columns to the data to store data that split columns will be stored

```sql 
ALTER TABLE Hotel
	ADD COLUMN Gender VARCHAR(10) AFTER `Gender/DOB`,
	ADD COLUMN Date_of_Birth Text AFTER `Gender`, 
	ADD COLUMN `Checkout_Date` TEXT AFTER `Checkout Date`;
```

 #### Extract relevant data from table columns such as `Gender/DOB` and `Checkout Date`. Time data will not be extracted because they are all 12am, which avoids redundant data and saves space. 
 
 - This query extacts Customers Gender, Date of Birth and Check out date from the aforementioned columns.

```sql 
SELECT SUBSTRING_INDEX(`Gender/DOB`, '/', 1) AS Gender, 	
	SUBSTRING_INDEX(SUBSTRING_INDEX(`Gender/DOB`, ' ', 1),'/', -3) AS Date_of_Birth,
	SUBSTRING_INDEX(`Checkout Date`, ' ', 1) AS Checkout_Date
FROM Hotel;
```

![image](https://github.com/user-attachments/assets/3d7452c0-fc82-4896-a61b-e2e724497815)

#### Inserted the extracted data into the earlier created columns:
```sql 
UPDATE Hotel
SET Gender = SUBSTRING_INDEX(`Gender/DOB`, '/', 1),	
    Date_of_Birth = SUBSTRING_INDEX(SUBSTRING_INDEX(`Gender/DOB`, ' ', 1),'/', -3),
    Checkout_Date = SUBSTRING_INDEX(`Checkout Date`, ' ', 1);
```
 
#### Check for blank columns in the dataset:
SELECT `Gender/DOB`, date_of_birth,
	SUBSTRING_INDEX(SUBSTRING_INDEX(SUBSTRING_INDEX(`Gender/DOB`, ' ', -3), ' ', 1), '/', -3) AS Correction
FROM `Hotel data`
WHERE Date_of_Birth = '';

#### There are 3 blank Date of Birth because there are leading spaces before them so I extracted the date and inserted the value into the blank spaces

![image](https://github.com/user-attachments/assets/aa83ab40-8b6a-4082-9ea2-5bb15525d910)

#### update the blank columns:

```sql 
UPDATE Hotel
SET Date_of_Birth = SUBSTRING_INDEX(SUBSTRING_INDEX(SUBSTRING_INDEX(`Gender/DOB`, ' ', -3), ' ', 1), '/', -3)
WHERE Date_of_Birth = '';
```

#### After extraction and updating new columns, all unwanted columns were dropped.

```sql 
ALTER TABLE hotel
	DROP COLUMN `Checkout Date`, 
      DROP COLUMN `Gender/DOB`;
```

## Further EDA and Data Cleaning by viewing distinct values in all columns to ascertain row datas are consistent.

```sql 
SELECT  DISTINCT gender
FROM hotel;
```
![Gender](https://github.com/user-attachments/assets/b23a6b30-c055-4c9b-95d9-d066144988c9)

#### There is a need to clean the gender column to ensure consistency


```sql 
SELECT  DISTINCT source
FROM hotel;
```

![source](https://github.com/user-attachments/assets/e4a9775e-2207-44cc-93f5-53d354f4c9a9)

#### Action to take:
- Trim word of mouth;
- Change organization**;
- Change 'Search***** engine'

```sql 
SELECT  DISTINCT purpose
FROM hotel;
```

![purpose](https://github.com/user-attachments/assets/86ba020a-a423-4483-86a5-0d18fc6db5cc)

#### Action to take:
- Change oth&er to other;
- Change bus to business;
- Change vac%ation to vacation.

```sql 
SELECT  DISTINCT feedback
FROM hotel;
```

![feedback](https://github.com/user-attachments/assets/51cfd120-dcd9-45d9-b644-4a0af12a0560)

#### Action to take:
- trim gym and change facility gym to gym

```sql 
SELECT  DISTINCT category
FROM hotel;
```

![category](https://github.com/user-attachments/assets/6828b9f1-db63-43ac-9017-c5184165cdc7)

#### Staffs needs to be changed to Staff.


#### There is no need to clean the following columns as they are unique and consistent:
- Guest Satisfaction
- NPS ratings
- facility ratings
- Full Name

### Clean out all identified inconsistent columns:
- Gender:

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

![image](https://github.com/user-attachments/assets/d14b1235-6f8a-416b-a3bf-b75f7ff334a7)

- Source:
  
```sql 
UPDATE Hotel
SET Source =  TRIM(Source);
```

```sql 
UPDATE Hotel
SET Source =  'Organization'
WHERE Source = 'Organization**';
```

```sql 
UPDATE Hotel
SET Source =  'Search engine'
WHERE Source = 'Search***** engine';
```

![image](https://github.com/user-attachments/assets/673f4ccb-4256-4534-a15c-47ec6b840b05)

- Purpose:

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

![image](https://github.com/user-attachments/assets/80d20792-ee96-45c6-b576-167603dfc2b3)

- Feedback:
  
```sql 
UPDATE Hotel
SET feedback =  TRIM(feedback);
```

```sql 
UPDATE Hotel
SET feedback =  'Gym'
WHERE feedback =  'Facility Gym';
```

![image](https://github.com/user-attachments/assets/69d86aad-3dbf-49ab-8974-165fb463774d)

- Category
  
```sql 
UPDATE Hotel
SET category =  'Staff'
WHERE category =  'Staffs';
```

![image](https://github.com/user-attachments/assets/f289b7da-2c1d-4390-9552-501bf3014ef9)

### Identify column data types for possible changes:
```sql 
DESCRIBE Hotel;
```

![image](https://github.com/user-attachments/assets/f29bd457-d06d-4ad7-8311-a29066fa9ffb)

- There is a need to change the data type of `Date_of_Birth` and `Checkout_Date` column from Text data type to Date type for the purpose of analysis

```sql 
SELECT Date_of_Birth, STR_TO_DATE(Date_of_Birth, '%m/%d/%Y') AS Changes,  Checkout_date, STR_TO_DATE(checkout_date, '%m/%d/%Y') AS Amended
FROM `Hotel`;
```

![image](https://github.com/user-attachments/assets/15dd5d6c-d8e9-44aa-9784-3dd3c59b3623)

- The above query changes string to date, these values will be updated to replace the two columns.
  
```sql 
UPDATE Hotel
SET Date_of_Birth = STR_TO_DATE(Date_of_Birth, '%m/%d/%Y'),  
	checkout_date = STR_TO_DATE(checkout_date, '%m/%d/%Y');
```

#### I changed data type of date of the two columns to date.

```sql 
ALTER TABLE Hotel
	MODIFY COLUMN Date_of_Birth DATE,
      MODIFY COLUMN Checkout_Date DATE;
```

![image](https://github.com/user-attachments/assets/fd1edd83-6470-4d67-9b4c-ac9be4861956)

#### Finally, I tried to identify if there are any duplicates in the data:

```sql 
SELECT ID, `Full Name`, Gender, Date_of_Birth, Checkout_Date, Purpose, Source,
	`Guest Satisfaction`, `NPS Ratings`, Feedback, Category, `Facility Ratings`, COUNT(*)
FROM hotel
GROUP BY ID, `Full Name`, Gender, Date_of_Birth, Checkout_Date, Purpose, Source,
	`Guest Satisfaction`, `NPS Ratings`, Feedback, Category, `Facility Ratings`
HAVING COUNT(*) > 1;
```

#### There are no duplicate within the data. This concludes the project of cleaning up this over 15, 000 row data, which is now ready for analysis. I hope you understood and enjoyed the process. Thank you for your time.


