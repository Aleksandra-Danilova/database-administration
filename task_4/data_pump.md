## Problem
From EPLOYEES and DEPARTMENTS tables of the HR scheme, transfer information about last and first names of employees, their salaries and department's name, where they work to another database.
Use external table (Data Pump).

## Solution

Create a directory object for both databases, for example, Student and ODMT.
![image](https://user-images.githubusercontent.com/76550825/166093663-c8d32bc6-9b58-4380-aa2c-43a1571408c2.png)

Working with the Student database, export the data to an export file: create an external table based on the physically existing EMPLOYEES and DEPARTMENTS tables.

![image](https://user-images.githubusercontent.com/76550825/166093803-cad77161-0215-4443-a4b2-b9a200722dba.png)

Grant read and write privileges from the directory to the previously created HR user of the ODMT database.

![image](https://user-images.githubusercontent.com/76550825/166093874-c90c5252-314c-408a-b294-6fab30202497.png)

![image](https://user-images.githubusercontent.com/76550825/166093877-27a263ba-60d2-4360-a266-37f99dbabe17.png)

Import data from the export file to the ODMT database.

![image](https://user-images.githubusercontent.com/76550825/166093885-3b406356-c94a-4a4b-8e10-434884754c0d.png)

Create an empty table with a structure, where the data will be be inserted.

![image](https://user-images.githubusercontent.com/76550825/166093904-f64291d5-bbf9-4b70-ba59-0a538dff721d.png)

Insert data:

![image](https://user-images.githubusercontent.com/76550825/166093922-8fe83881-2e74-4137-8172-44c947da9ab0.png)

Directory files:

![image](https://user-images.githubusercontent.com/76550825/166093930-f9ec2bf6-5d3f-445a-b3c2-fba24a3b7213.png)


## Testing

Let's see the results.

1.
![image](https://user-images.githubusercontent.com/76550825/166093946-009d6b4c-7710-4df8-9c59-fba51a6588b1.png)

![image](https://user-images.githubusercontent.com/76550825/166093951-5d525d84-fe9d-45c6-bf54-1a6def04be86.png)

2.
![image](https://user-images.githubusercontent.com/76550825/166093953-9cb9cdaa-48a9-46e5-8125-d4c5104e1705.png)

3.
![image](https://user-images.githubusercontent.com/76550825/166093959-9e76987a-1518-485d-b6c2-dbfc430ef951.png)

