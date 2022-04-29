## Problem
Organize a user audit that allows you to control the execution of DDL operations, with the results saved in the operating system audit file.
Test the operation of the system when executing various commands by different users.

## Solution
We will perform a standard audit to control DDL operations.
By the task's condition, the results should be stored in the operating system audit file.
Therefore, we will set the value of the AUDIT_TRAIL parameter to the OS.

<img src="https://user-images.githubusercontent.com/76550825/166075520-ddfbfb0f-e2de-4e38-9b74-24ab69810797.png" width="624" height="20">

Restart the database because the static parameter was changed.

<img src="https://user-images.githubusercontent.com/76550825/166075612-bc1a6b58-1e8b-46d9-900f-07d8006c6b43.png" width="307" height="193">

<img src="https://user-images.githubusercontent.com/76550825/166075620-577ace41-9f57-47a3-b015-5474d4d2d54e.png" width="273" height="97">

Select DDL operations for auditing.
Moreover, in the list of available statements there are such as, for example, TABLE and CLUSTER, which in general includes CREATE, ALTER, DROP, and TRUNCATE.

<img src="https://user-images.githubusercontent.com/76550825/166075744-c8c85035-f117-4f23-b181-c5f33267e73c.png" width="611" height="318">


## Testing

To view audit records, use Event Viewer -> Windows Logs -> Application.
In Windows, when AUDIT_TRAIL = OS, audit records are recorded as events in Event Viewer, log files are located in C:\Windows\System32\winevt\Logs.

<img src="https://user-images.githubusercontent.com/76550825/166075833-fab8c8e7-fd43-4cb6-9f2c-dc6950af1d41.png" width="624" height="432">

<b>1.</b> Changing the table (HR):

<img src="https://user-images.githubusercontent.com/76550825/166075918-605a5273-2771-47c6-a39f-34313c3098fc.png" width="211" height="80">

<img src="https://user-images.githubusercontent.com/76550825/166077104-442b4530-a0a3-41b0-86eb-13b1c1814029.png" width="386" height="224">



<b>2.</b> Creating a sequence (SH):

<img src="https://user-images.githubusercontent.com/76550825/166075966-6db25bcf-8e1c-47c1-9652-1acfa26a2abf.png" width="245" height="101">

<img src="https://user-images.githubusercontent.com/76550825/166075981-14b41566-3b3a-4d82-9095-4e65ffd93af1.png" width="365" height="212">

Deleting a sequence:

<img src="https://user-images.githubusercontent.com/76550825/166076013-39528ce5-5958-45f7-8fc8-9cb55a35cf27.png" width="230" height="52">

<img src="https://user-images.githubusercontent.com/76550825/166076020-3b89a1e1-69ce-4566-b2f0-ad40384327dc.png" width="365" height="212">

Creating a comment to the CUST_LAST_NAME column of the CUSTOMERS table:

<img src="https://user-images.githubusercontent.com/76550825/166076067-b95cc3a6-37c3-4a46-8ae2-01a6d96fde67.png" width="587" height="53">

<img src="https://user-images.githubusercontent.com/76550825/166076075-0b6767ab-def2-4737-8ad6-2f57609c31c8.png" width="393" height="224">



<b>3.</b> Creating a table (DBA1):

<img src="https://user-images.githubusercontent.com/76550825/166076098-34f65a0e-bc3a-4200-a658-88e2bc665603.png" width="204" height="89">

<img src="https://user-images.githubusercontent.com/76550825/166076101-6041234e-7fd7-4fad-bd82-762af91d9257.png" width="364" height="209">


Let's clean up the PRODUCT_MASTER table in the INVENTORY tablespace, so that there is no error, we will omit the primary key through the Enterprise Manager:

<img src="https://user-images.githubusercontent.com/76550825/166076145-1586fae4-3e1b-4cf0-8674-10b6e2513bc9.png" width="332" height="43">

<img src="https://user-images.githubusercontent.com/76550825/166076151-9f37e5ab-0e98-402b-b662-9589e95cab84.png" width="432" height="247">






