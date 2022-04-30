## Problem

ASMCMD: Perform backup and restore of a disk group using metadata (md_backup, md_restore).

## Solution

ASMCMD is a command-line utility that allows you to perform some operations related to ASM.

For example,
 1) *md_backup* saves metadata about the disk:
    - templates
    - names of disks, disk groups, failure groups
    - user-created directories
    - information on disk group compatability
   
 2) *md_restore* restores metadata about the disk:
    - full
    - nodg (without restoring the disk group)
    - newdg (restoring metadata to a new disk group)
 
 3) *lsdsk* displays a list of disk information
   
Set the value of the environment variable ORACLE_SID:

![image](https://user-images.githubusercontent.com/76550825/166098194-62e967bd-5387-4d74-8bd9-7b82a4dbc98f.png)

Connect to SQL\*Plus and check that we are connected to the +ASM instance.

To avoid the following errors later when restoring, we will upgrade the version of compatibility attributes:

![image](https://user-images.githubusercontent.com/76550825/166099741-16b5a9a1-4755-49f4-b8d5-540221438fa3.png)

![image](https://user-images.githubusercontent.com/76550825/166099748-8408b813-268a-498a-a2dd-6e634372fcf9.png)

![image](https://user-images.githubusercontent.com/76550825/166099751-5ff8f4b8-62b1-4dc9-a094-c4f14ef37ad4.png)

Exit SQL\*Plus and connect to ASMCMD:

![image](https://user-images.githubusercontent.com/76550825/166099759-a34bc632-e9c1-496f-ad37-692011e5bc78.png)

Perform a backup using the md_backup command. This command creates a backup file containing metadata for the DATA disk group.

![image](https://user-images.githubusercontent.com/76550825/166099772-881b1338-e59f-4df1-9afe-dceb6a1a6e49.png)

![image](https://user-images.githubusercontent.com/76550825/166099776-78122447-41d9-459a-9518-ede996b40946.png)

Content of the block1_backup file:

![image](https://user-images.githubusercontent.com/76550825/166099807-bf5e0cb3-077c-4413-a2c8-9b2dcfe9ff36.png)

Connect back to SQL\*Plus.
To avoid the errors presented below, first stop the database (it is named Student) that uses this disk group and perform unmounting of the DATA disk group:

![image](https://user-images.githubusercontent.com/76550825/166099831-e643e775-d077-4639-ba1d-3c94e737b908.png)

![image](https://user-images.githubusercontent.com/76550825/166099832-f23e699c-214a-4da3-886a-f8b90e4a05fa.png)

Then delete the disk group itself, including INCLUDING CONTENTS, since the disk group contains files with metadata.

![image](https://user-images.githubusercontent.com/76550825/166099871-faf3b659-497c-4692-a711-55ba58c5ab9b.png)

Return to ASMCMD and check that there are no more disks of the DATA group with the lsdsk command.

![image](https://user-images.githubusercontent.com/76550825/166099928-ec88d5f8-8cf0-40f4-8078-4326519a9098.png)

Perform a full restore with the md_restore command.

![image](https://user-images.githubusercontent.com/76550825/166099940-a4618159-8bb3-48e4-9a31-38cf6db2cc40.png)

The disks of the DATA group appeared in the list:

![image](https://user-images.githubusercontent.com/76550825/166099951-0da5adf4-7fe9-46fe-a064-5789ddff17e7.png)
