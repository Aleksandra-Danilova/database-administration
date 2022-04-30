## Problem

Perform TSPITR with RMAN using an Auxiliary Instance managed by the RMAN utility.
Provide the use of RMAN debugging and operation tracking mode.
Analyze the operations performed by RMAN when solving the problem.

## Solution

Let's export the HR schema of the ORCL database.

![image](https://user-images.githubusercontent.com/76550825/166102868-b866589e-1ca9-4ce6-9bc0-3d31dd28de2a.png)

Let's create the HR TEST tablespace.

![image](https://user-images.githubusercontent.com/76550825/166102881-2cbb90bb-3524-4e4c-aef8-3034d0a51745.png)

Create HRTEST user and grant this user privileges.

![image](https://user-images.githubusercontent.com/76550825/166102920-c3fc2076-77e3-48b8-975c-c0c60dad20d0.png)

Create the parameters file and perform the import.

![image](https://user-images.githubusercontent.com/76550825/166102935-62c25869-1329-4be2-a3cc-af4d8d18cc09.png)

![image](https://user-images.githubusercontent.com/76550825/166102938-532f7e35-b821-4ee1-ab51-69fbbee5eb23.png)

Connect to the target ORCL database and RCAT recovery catalog with debug and trace options.

![image](https://user-images.githubusercontent.com/76550825/166102951-734c71f2-6379-4c07-913d-5bada1f4cdc5.png)

![image](https://user-images.githubusercontent.com/76550825/166102956-47eeaa85-3713-4b81-9bcb-1479d37b600d.png)

Example of a trace file's contents:

![image](https://user-images.githubusercontent.com/76550825/166102983-b79f575a-1070-4b48-83af-bbf03f3ca678.png)

Example of a debug file's contents:

![image](https://user-images.githubusercontent.com/76550825/166103013-7a9e034c-7227-490a-bc89-741fe8dc2f6e.png)

Perform a backup.

![image](https://user-images.githubusercontent.com/76550825/166103020-957b4795-f567-4bdc-8e05-f1ec10595932.png)

![image](https://user-images.githubusercontent.com/76550825/166103022-9b4a62a0-56df-4b0e-b5e7-02523f792bf0.png)

Let's find out the current SCN.

![image](https://user-images.githubusercontent.com/76550825/166103027-134fac5c-2c35-48a9-ba74-53aab50f18b2.png)

Let's make changes to the EMPLOYEES table by transferring employees from 60 department to 200.

![image](https://user-images.githubusercontent.com/76550825/166103047-85b03bfd-721e-4c6d-ba3f-df82d01ac0d6.png)

Let's check that the changes have been actually applied.

![image](https://user-images.githubusercontent.com/76550825/166103084-5c02c788-625f-4385-b4e6-931888e4ce0d.png)

Let's first create a catalog for auxiliary destination.

![image](https://user-images.githubusercontent.com/76550825/166103092-86d672ef-98f9-45bf-86ac-6488e2bb8131.png)

There are no dependencies interfering with the execution of TSPITR.

![image](https://user-images.githubusercontent.com/76550825/166103100-28a04e66-292d-4978-b316-c8f1f6189457.png)

Recover HRTEST tablespace to SCN=1356129.

![image](https://user-images.githubusercontent.com/76550825/166103118-c16f5a61-b8c0-46a8-b38d-35502d75f5e1.png)

1) An automatic instance is created with SID='tspitr' with the initialization parameters shown below.

![image](https://user-images.githubusercontent.com/76550825/166103131-8a01293c-f0fb-4cf2-a4f1-982b9e2dc21d.png)

The instance is being started.

It is checked using the TRANSPORT_SET_CHECK procedure of the DBMS_TTS package whether the set of tablespaces of recovery set (for migration) is autonomous.

![image](https://user-images.githubusercontent.com/76550825/166103183-e6370678-824d-4b0d-a882-1686dc11f970.png)

2) Restoring the tablespace offline in the target database; the backup control file from the time preceding the target time to the auxiliary instance.

![image](https://user-images.githubusercontent.com/76550825/166103231-c8177118-8280-4c6f-86d8-d2563f2a7f98.png)

Restoring data files from the recovery set and the auxiliary set to the auxiliary instance.

Files are restored to auxiliary destination.

![image](https://user-images.githubusercontent.com/76550825/166103246-f08bab9b-5d50-4b31-b7ae-b066b49acb80.png)

![image](https://user-images.githubusercontent.com/76550825/166103252-5a7d14c8-7f52-40e2-b684-4c477616d56a.png)

3) Recovery of the restored data files in the auxiliary instance to the specified SCN.

![image](https://user-images.githubusercontent.com/76550825/166103307-886bdfaf-035d-42cf-863b-bea619b08b0d.png)

4) Opening the auxiliary database with the RESETLOGS option.

![image](https://user-images.githubusercontent.com/76550825/166103316-f2954aaa-f93d-4c76-8b9c-02ce6488c9b5.png)

5) Make the tablespaces of the recovery set to be read-only in the auxiliary instance.

![image](https://user-images.githubusercontent.com/76550825/166103342-af2ef0cf-e547-4ede-87a1-b1debf8f025d.png)

6) Export the recovery set tablespace from an auxiliary instance using the Data Pump utility to create a transportable tablespace dump file.

![image](https://user-images.githubusercontent.com/76550825/166103391-7678be14-7160-4405-b25b-e5b117f7c380.png)

7) Shutdown of the auxiliary instance.

![image](https://user-images.githubusercontent.com/76550825/166103399-d69ad16f-5b09-4447-9aa9-19b7ab68b89f.png)

8) Deleting the recovery set tablespace from the target object.

![image](https://user-images.githubusercontent.com/76550825/166103428-bc77860e-aea9-4232-9fa5-19dfe2e76398.png)

9) Data Pump utility reads the dump file of the transportable tablespace and plugs the tablespaces of the recovery set to the target object.

![image](https://user-images.githubusercontent.com/76550825/166103461-c1a18e98-6842-4b65-bc5a-500321b4b546.png)

10) Reads/writes the tablespace that has been placed in the target database and immediately takes it offline.

![image](https://user-images.githubusercontent.com/76550825/166103488-bb901764-1333-4619-879a-5c4d52e05725.png)

11) Deleting all files of the auxiliary set.

![image](https://user-images.githubusercontent.com/76550825/166103500-8b9c4c5f-878d-41c1-985e-ed8dbe4fcffc.png)

We transfer the tablespace to ONLINE and check that the employees have returned back to the 60th department.

![image](https://user-images.githubusercontent.com/76550825/166103523-f767a5dc-b1c3-43af-a2b2-4c52639c9b2b.png)


