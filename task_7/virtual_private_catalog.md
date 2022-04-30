## Problem
1. Create a Test database.
2. Create a virtual catalog to store Test database backups' metadata.
3. Create a virtual catalog to store Oracle database backups' metadata.
4. Demonstrate the work of the catalog and virtual catalogs.

## Solution

Create database named Test.

<img src="https://user-images.githubusercontent.com/76550825/166100275-cd001f38-15e3-4da1-9b24-30d3012af71a.png" width="497" height="147">

<img src="https://user-images.githubusercontent.com/76550825/166100294-13eab252-efa5-4904-8faf-18a225a8b88d.png" width="391" height="171">

Create RCAT tablespace for storing repository data.

![image](https://user-images.githubusercontent.com/76550825/166100624-b40c4593-1389-40c5-a4fc-bf9533dc20b9.png)

Create RCATOWNER, TESTOWNER, STUDOWNER users by granting the RECOVERY_CATALOG_OWNER privilege,
where RCATOWNER is the owner of the main recovery catalog, TESTOWNER and STUDOWNER are the owners of virtual private catalogs.

![image](https://user-images.githubusercontent.com/76550825/166100324-768140b2-0ace-49ca-b666-5982b76ee1c6.png)

Create a recovery catalog.

![image](https://user-images.githubusercontent.com/76550825/166100636-4924ee13-e80b-4d4c-9437-2eea5e860f79.png)

Register Test database in it.

Then grant the CATALOG FOR DATABASE privilege so that the user can view the database backup storage through the virtual catalog and the new owner of the virtual recovery catalog can work with the database.

Having connected to the recovery catalog as TESTOWNER, we will create a virtual catalog with the CREATE VIRTUAL CATALOG command.

Check that there are no other databases besides the Test database.

![image](https://user-images.githubusercontent.com/76550825/166100413-b7cc83e9-6621-4ae0-82ac-3925d408935b.png)

Perform similar operations for the second database, for example, named Student:

![image](https://user-images.githubusercontent.com/76550825/166100444-18d90860-c150-4e3d-940b-fa3b7f2bb26b.png)

Having connected as RCATOWNER, we see that two databases are registered in the catalog.

![image](https://user-images.githubusercontent.com/76550825/166100454-49dd6871-08dc-4c46-8860-d6a57853d39e.png)

Let's try to withdraw from TESTOWNER the privilege to register new databases in a virtual private catalog.

![image](https://user-images.githubusercontent.com/76550825/166100474-86c81705-fc61-4092-8d24-d80c714cae40.png)

Hence, an error occurs.

![image](https://user-images.githubusercontent.com/76550825/166100486-9e8e4c2f-790b-4e1e-984d-8416fe331ce1.png)

Also, try to back up the Test database by first switching it to Archive mode:

![image](https://user-images.githubusercontent.com/76550825/166100508-a3812847-a363-4dc3-bc95-ffb8c43f45b7.png)

![image](https://user-images.githubusercontent.com/76550825/166100517-33ad9d0d-1282-48ed-a348-51c5c70cc4e5.png)

Similarly for Student:

![image](https://user-images.githubusercontent.com/76550825/166100537-93caa94f-5fa6-4e9d-9b34-d05ed070cf55.png)

![image](https://user-images.githubusercontent.com/76550825/166100538-c21f03ba-deca-4d6e-8ed9-0612128d9eb4.png)

Similarly for owner RCATOWNER:

![image](https://user-images.githubusercontent.com/76550825/166100557-4494b823-a311-415f-b90e-bc325e4dfcb6.png)

![image](https://user-images.githubusercontent.com/76550825/166100559-01c074f0-298f-4f7f-992e-a79208119df0.png)

