## Problem

1. Change the database name from ORCL to ORCL_REN without changing the DBID.
2. Create a 4-section database backup with dual-mode encryption and optimization.
3. Create a 4-section database backup with dual-mode encryption without optimization.
4. Confirm the availability of backups with the required parameters.
5. Restore the database from the created backups.

## Solution

After connection to the ORCL database check its DBID.

![image](https://user-images.githubusercontent.com/76550825/166101669-79b7d932-3e98-4140-9e99-0e418d50762a.png)

Shutdown the database and mount it.

![image](https://user-images.githubusercontent.com/76550825/166101707-cf1dffdf-67d5-41f4-b635-64c30ae8f20e.png)

Using the DBNEWID utility, change the DBNAME database name to ORCL_REN and set the SETNAME parameter, which indicates that DBNEWID should change the database name, but should not change the DBID (by default, NO).
And also specify the file for recording utility messages.
By default, the utility overwrites the previous log file.

![image](https://user-images.githubusercontent.com/76550825/166101736-5864fcd5-9f0f-48af-955f-ddab348115e2.png)

After reviewing the contents of this log file, we see that the database name has been changed successfully.

![image](https://user-images.githubusercontent.com/76550825/166101743-e84a16ca-85ca-4f12-92d2-8b841853bc81.png)

Update the database name in the spfile with the ALTER SYSTEM SET command.

![image](https://user-images.githubusercontent.com/76550825/166101753-d5745a6e-8d9c-43fd-b2e2-3e4243f830e3.png)

Create a new password file using the ORAPWD utility.

![image](https://user-images.githubusercontent.com/76550825/166101761-e29707e9-0280-4798-993b-2137379de40d.png)

![image](https://user-images.githubusercontent.com/76550825/166101764-be3e5b3e-b3cd-4b88-8e1c-d2083cdf2af5.png)

Add information about ORCL_REN to the Net Manager, change ORCL to ORCL_REN in the tnsnames.ora file and restart the listener.

![image](https://user-images.githubusercontent.com/76550825/166101787-8e0e0a63-22cf-4a27-8a91-7da7b2b46cf6.png)

![image](https://user-images.githubusercontent.com/76550825/166101793-25a67dab-ca0d-41d3-a6a0-49b1c1938a99.png)

![image](https://user-images.githubusercontent.com/76550825/166101799-799cf6fa-1420-498a-8936-9c5cd69d32b6.png)

Let's check that it is possible to connect to the database, and also find out the DBID. The DBID has not changed.

![image](https://user-images.githubusercontent.com/76550825/166101820-79be243b-63ff-4002-a6c2-6a20aaa62ff5.png)

Next, create a 4-section backup of the database with dual-mode encryption and optimization.

First, enable optimization with the CONFIGURE BACKUP OPTIMIZATION ON command. It skips file backups when similar files have already been reserved. A backup will not be made if the backup has already been created.

![image](https://user-images.githubusercontent.com/76550825/166101841-40f15158-3a8f-47f6-89f7-9ee5482f3956.png)

Let's check that the parameter is set.

![image](https://user-images.githubusercontent.com/76550825/166101848-224c4892-2d61-4fb1-8424-b75c87986e15.png)

Before enabling encryption, you need to create a folder for the wallet in the directory $ORACLE_BASE/admin/$ORACLE_SID/wallet and the wallet itself.

![image](https://user-images.githubusercontent.com/76550825/166101860-b8712573-78ef-4d1a-a59a-7775980636f1.png)

![image](https://user-images.githubusercontent.com/76550825/166101863-2e159c97-7392-4e6a-adce-341718665bb6.png)

![image](https://user-images.githubusercontent.com/76550825/166101867-052147a1-14fe-46ec-a853-8fbc606e60a6.png)

Set up the environment so that all RMAN backups are encrypted:

![image](https://user-images.githubusercontent.com/76550825/166101882-5af1d115-598b-40e6-a64f-50e2259b8a2f.png)

And enable dual-mode encryption with the command SET THE ENCRYPTION DEFINED BY oracle123 ENABLED FOR ALL TABLESPACES

![image](https://user-images.githubusercontent.com/76550825/166101907-b4d961f9-23c3-4024-8c43-e86d84034acf.png)

Check that encryption is enabled:

![image](https://user-images.githubusercontent.com/76550825/166101912-ef87bbcc-f47e-427d-a59b-0d6d60159c2a.png)

Let's look at all the parameters together with the SHOW ALL command:

![image](https://user-images.githubusercontent.com/76550825/166101920-22f5f32c-f83b-46ef-8d57-1c76fc5fb4ed.png)

The size of the data files is 2000 MB.

![image](https://user-images.githubusercontent.com/76550825/166101938-5aec29d5-935a-4e01-b64f-8dff3de1a87a.png)

So, for a 4-section backup, 2000 / 4 = 500 MB will be required.

Run the BACKUP SECTION SIZE 500M DATABASE command.

![image](https://user-images.githubusercontent.com/76550825/166101954-60dd4720-e082-4bdc-9c0c-2b5a43f13044.png)

It took 1 min 21 sec to perform a backup.

![image](https://user-images.githubusercontent.com/76550825/166101962-443a5421-5e1e-4d74-a84c-4f27c7e046cb.png)

Disable optimization.

![image](https://user-images.githubusercontent.com/76550825/166101969-6666162f-1c19-4d50-b678-3955e7bd3662.png)

Check parameters.

![image](https://user-images.githubusercontent.com/76550825/166101982-1fd36ca9-7d47-432b-b3ac-a789f0f3519e.png)

Perform backup with encryption and without optimization.

![image](https://user-images.githubusercontent.com/76550825/166101991-21a81d90-9696-434f-886b-bd617aab14c3.png)

![image](https://user-images.githubusercontent.com/76550825/166101996-35d9f199-4604-4b83-95f7-f5c0a5ef9e9d.png)

It took 2 min 6 sec to perform a backup.

Perform restore from backups.

To do this, get the database to the MOUNT state.

Then install decryption.

![image](https://user-images.githubusercontent.com/76550825/166102017-2ab014b2-49c8-4bca-9474-ab439386a511.png)

Perform RESTORE DATABASE.

![image](https://user-images.githubusercontent.com/76550825/166102030-b33d90bb-6bad-420b-9005-9bf9fe60e64d.png)

And then the RECOVER command.

![image](https://user-images.githubusercontent.com/76550825/166102044-d5911252-f9ec-438d-bd6e-33af7fbe77da.png)

At the end, open the database.

![image](https://user-images.githubusercontent.com/76550825/166102051-3d2c2e15-1658-4af9-a198-048881f2b017.png)


