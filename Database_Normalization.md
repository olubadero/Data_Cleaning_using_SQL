### This project is to create mulitple lookup and dimension table from the cleaned Hotel Table. The purpose of this is to reduce redundancies, save space within the Database. There are multiple columns within the data that are duplicated and can be extracted as lookup values for the Facts of the table.

#### I began the project by creating unique identifying column ID for certain columns in the table that had none. These IDs will replace these columns in the final facts table.

#### Create Source_ID and Source table

```sql
SELECT distinct CASE WHEN Source = 'Hotel Booking Sites' THEN 1
	WHEN Source = 'Internet Advertisement' THEN 2
	WHEN Source = 'Newspaper' THEN 3
	WHEN Source = 'Organization'THEN 4
	WHEN Source = 'Search Engine' THEN 5
	WHEN Source = 'Television Advertisement' THEN 6
	WHEN Source = 'Word of Mouth' THEN 7 END AS Source_ID, source
FROM Hotel
ORDER BY  Source_ID;
```

```sql
CREATE TABLE Source_Table AS 
	SELECT distinct CASE WHEN Source = 'Hotel Booking Sites' THEN 1
		WHEN Source = 'Internet Advertisement' THEN 2
		WHEN Source = 'Newspaper' THEN 3
		WHEN Source = 'Organization'THEN 4
		WHEN Source = 'Search Engine' THEN 5
		WHEN Source = 'Television Advertisement' THEN 6
		WHEN Source = 'Word of Mouth' THEN 7 END AS Source_ID, source
	 FROM Hotel
	 ORDER BY  Source_ID;
```

![Source Table](https://github.com/user-attachments/assets/73b5e8a2-aaff-40a4-a9e7-35927b163d1b)

#### Create Purpose_ID and Purpose table

```sql
SELECT distinct CASE WHEN  Purpose = 'Business' THEN 1
		WHEN  Purpose = 'Function' THEN 2
		WHEN  Purpose = 'Other' THEN 3
		WHEN  Purpose = 'Vacation'THEN 4 END AS Purpose_ID, Purpose
FROM Hotel
ORDER BY  Purpose_ID;
```

```sql
CREATE TABLE Purpose_Table AS 
	SELECT distinct CASE WHEN  Purpose = 'Business' THEN 1
		WHEN  Purpose = 'Function' THEN 2
		WHEN  Purpose = 'Other' THEN 3
		WHEN  Purpose = 'Vacation'THEN 4 END AS Purpose_ID, Purpose
	 FROM Hotel
	 ORDER BY  Purpose_ID;
```

![Purpose Table](https://github.com/user-attachments/assets/76a1b682-8543-4801-906a-2e7760676c2e)


#### Create Category_ID and Category table

```sql
SELECT distinct CASE WHEN  category = 'Facility' THEN 1
		WHEN  category = 'Restaurant' THEN 2
		WHEN  category = 'Room' THEN 3
		WHEN  category = 'Staff'THEN 4 END AS Category_ID, Category
FROM Hotel
ORDER BY  Category_ID;
```

```sql
CREATE TABLE Category_Table AS 
	SELECT distinct CASE WHEN  category = 'Facility' THEN 1
		WHEN  category = 'Restaurant' THEN 2
		WHEN  category = 'Room' THEN 3
		WHEN  category = 'Staff'THEN 4 END AS Category_ID, Category
	 FROM Hotel
	 ORDER BY  Category_ID;
```

![category_table](https://github.com/user-attachments/assets/3f11f67b-32e2-46ee-b0d0-9e9986da831e)

#### Create Feedback_ID and Feedback Table

```sqsl
SELECT DISTINCT   
		CASE WHEN  feedback = 'Broadband & TV' THEN 1
			WHEN  feedback = 'Check-in Process' THEN 2
			WHEN  feedback = 'Food Quality' THEN 3
			WHEN  feedback = 'Gym' THEN 4
			WHEN  feedback = 'Room Cleanliness' THEN 5
			WHEN  feedback = 'Room Service' THEN 6
			WHEN  feedback = 'Staff Attitude' THEN 7
			WHEN  feedback = 	'Variety of Food' THEN 8 END AS Feedback_ID,  Feedback
FROM Hotel
ORDER BY Feedback_ID;
```

```sql
CREATE TABLE Feedback_Table AS
	SELECT DISTINCT   
		CASE WHEN  feedback = 'Broadband & TV' THEN 1
			WHEN  feedback = 'Check-in Process' THEN 2
			WHEN  feedback = 'Food Quality' THEN 3
			WHEN  feedback = 'Gym' THEN 4
			WHEN  feedback = 'Room Cleanliness' THEN 5
			WHEN  feedback = 'Room Service' THEN 6
			WHEN  feedback = 'Staff Attitude' THEN 7
			WHEN  feedback = 	'Variety of Food' THEN 8 END AS Feedback_ID,  Feedback
	FROM Hotel
	ORDER BY Feedback_ID;
```      

![Feedback table](https://github.com/user-attachments/assets/575da2a7-9dec-46a7-bde7-1dec11cb340e)

#### I went further to extract multiple columns within the facts table that already had unique identifying fields and created their tables


#### Create Satisfaction table
```sql
CREATE TABLE Satisfaction_Table AS
		SELECT DISTINCT `Guest Satisfaction`, Satisfaction_Metric 
		FROM Hotel
		ORDER BY `Guest Satisfaction`;
```

![Satisfaction table](https://github.com/user-attachments/assets/1911f6c6-525d-4640-912b-fdce03f4162e)

#### Create NPS table
```sql
CREATE TABLE NPS_Score AS
	SELECT DISTINCT `NPS Ratings`, NPS_Metric 
	FROM Hotel
	ORDER BY `NPS Ratings`;
```

![image](https://github.com/user-attachments/assets/48c2b436-ea5c-4d7a-ad18-eb335e368dec)

#### Create Facility_Ratings table
```sql
CREATE TABLE Facility_Ratings AS      
	SELECT DISTINCT `Facility Ratings`, Ratings_Metric
	FROM Hotel
	ORDER BY `Facility Ratings`;
```

![Facility Ratings](https://github.com/user-attachments/assets/b528428c-1ca5-4fbd-a1eb-bb2bbd90b4c0)

#### Create customer table
```sql
CREATE TABLE Guest_Details AS
		SELECT DISTINCT `Full Name`, Gender, Date_of_Birth, Age, Age_Group
		FROM Hotel
		ORDER BY `Full Name`;
 ```           

![image](https://github.com/user-attachments/assets/839b39be-da8c-425c-b493-064f5cf70742)

#### Having extracted and created all lookup and dimension table from th original table. I created a more streamlined facts table, which will be used for analysis.

```sql
CREATE TABLE Hotel_Facts AS
SELECT Hotel_ID,`Full Name`, Checkout_Date, `Guest Satisfaction`, `NPS Ratings`,`Facility Ratings`,
	CASE WHEN Source = 'Hotel Booking Sites' THEN 1
	WHEN Source = 'Internet Advertisement' THEN 2
	WHEN Source = 'Newspaper' THEN 3
	WHEN Source = 'Organization'THEN 4
	WHEN Source = 'Search Engine' THEN 5
	WHEN Source = 'Television Advertisement' THEN 6
	WHEN Source = 'Word of Mouth' THEN 7 END AS Source_ID,
      
   CASE WHEN  Purpose = 'Business' THEN 1
		WHEN  Purpose = 'Function' THEN 2
		WHEN  Purpose = 'Other' THEN 3
		WHEN  Purpose = 'Vacation'THEN 4 END AS Purpose_ID,
            
      CASE WHEN  category = 'Facility' THEN 1
		WHEN  category = 'Restaurant' THEN 2
		WHEN  category = 'Room' THEN 3
		WHEN  category = 'Staff'THEN 4 END AS Category_ID,
            
	CASE WHEN  feedback = 'Broadband & TV' THEN 1
			WHEN  feedback = 'Check-in Process' THEN 2
			WHEN  feedback = 'Food Quality' THEN 3
			WHEN  feedback = 'Gym' THEN 4
			WHEN  feedback = 'Room Cleanliness' THEN 5
			WHEN  feedback = 'Room Service' THEN 6
			WHEN  feedback = 'Staff Attitude' THEN 7
			WHEN  feedback = 	'Variety of Food' THEN 8 END AS Feedback_ID
FROM Hotel;
```

#### Here is the final facts table:

![image](https://github.com/user-attachments/assets/f1cd7985-53dd-44b7-af6d-aed461a1b53e)
     
There is a need to change the data type of newly created column IDs:

```sql
DESCRIBE Hotel_Facts;
```

![data type](https://github.com/user-attachments/assets/565ff293-fd61-4c33-8067-0a4bc613b6d5)

```sql
ALTER TABLE hotel_facts
  	MODIFY COLUMN Source_ID VARCHAR(3),
  	MODIFY COLUMN Purpose_ID VARCHAR(3),
    MODIFY COLUMN Category_ID VARCHAR(3),
    MODIFY COLUMN Feedback_ID VARCHAR(3);
```

![data type final](https://github.com/user-attachments/assets/cb7ef736-3e14-40a3-af67-47f54cd8b789)

#### This marks the end of all data cleaning, transformation and it is ready for analysis. Thank you. 
      
