## Problem

1. Create an ODMT database with a USERS tablespace managed by dictionary.
2. Using EM, create a dictionary-managed TSD tablespace with two files with increasing extents. When creating, use OMF technology.
3. Display information about the types of controls and characteristics of tablespaces.
4. Move the EMPLOYEES and DEPARTMENTS tables with links between them from the ORCL database to the TSD table space of the ODMT database using the Db_link object.
5. Convert the USERS tablespace to a locally managed tablespace.
6. Create a locally managed TSL tablespace with a fixed file size (100 M) by moving the DEPARTMENTS table to it.
7. Convert the TSL tablespace into a dictionary-driven tablespace.
8. Set extent management to Automatic mode. Get information about the extent sizes in the Departments segment.
9. Display information on the USERS, TSL, TSD.
10. Check the operability of the DEPARTMENTS and EMPLOYEES tables by executing a query displaying the names of employees and the names of departments in which they work.

## Solution
<b>1.</b> Create ODMT database:

<img src="https://user-images.githubusercontent.com/76550825/166067266-2d6532c8-83e6-4ecc-9fde-859974902cbd.png" width="443" height="307">
<img src="https://user-images.githubusercontent.com/76550825/166067920-18b9a36d-0b33-4dd5-a8e1-2b3451606f3a.png" width="443" height="307">

Use OMF:

<img src="https://user-images.githubusercontent.com/76550825/166068009-df2519e8-4840-4da4-9179-863933b2d12d.png" width="499" height="350">

Make the USERS tablespace manageable by dictionary:

<img src="https://user-images.githubusercontent.com/76550825/166068212-e2ee6f13-14ed-4c9c-aefa-97fcd47d9587.png" width="497" height="349">

Also, make the SYSTEM tablespace manageable by dictionary:

<img src="https://user-images.githubusercontent.com/76550825/166068320-cc6b0647-5f7e-4a19-bf56-d495b7e9eede.png" width="495" height="282">



<b>2.</b> Using EM, we will create a dictionary-managed TSD tablespace with two files with an increasing extent size.

Because the first file cannot be created using OMF technology (it is impossible to create a tablespace without it) and it cannot be deleted either, so we will not create it using OMF, and the next two can already be created using OMF.

Check the box to automatically enlarge the file.

<img src="https://user-images.githubusercontent.com/76550825/166068535-58751bf3-98a1-4719-9344-860ad893b211.png" width="237" height="35">
<img src="https://user-images.githubusercontent.com/76550825/166068544-3cb84a53-34d5-41f9-b02a-f1567d39bd56.png" width="590" height="343">



<b>3.</b> Let's look at the information about the types of management and characteristics of tablespaces:
<img src="https://user-images.githubusercontent.com/76550825/166068677-f3b1d950-d6ac-4736-b9d9-6f0685901a3f.png" width="598" height="134">
<img src="https://user-images.githubusercontent.com/76550825/166068693-78415ea6-d7c2-4a79-ae92-c0274111713e.png" width="504" height="180">



<b>4.</b> Move the EMPLOYEES and DEPARTMENTS tables with links between them from the ORCL database to the TSD table space of the ODMT database using the Db_link object.

Create a Db_link object that will link ODMT and ORCL databases.

<img src="https://user-images.githubusercontent.com/76550825/166069038-53a2a0cb-5c5c-4738-aa81-40ff48a330fd.png" width="404" height="237">

To move tables, we will use the utilities data pump export / import, this technology allows you to quickly move data between databases.

First, you need to grant the user privileges so that the user can work with a specific directory and have access to the object of this directory. By default, the creation of a directory is available only to privileged users (the DATA_PUMP_DIR directory is the default directory and is created when creating a database). Therefore, we will perform the export under the SYSTEM account.

<img src="https://user-images.githubusercontent.com/76550825/166069323-98b82537-ed8d-4e63-a006-1c3562b55abd.png" width="445" height="42">

You can export the entire HR schema to the TSD tablespace of the ODMT database.

The Data Pump Export utility supports the ability to export data from a "remote" database. We export using the network_link parameter.

<i>expdp system/oracle@odmt dumpfile=file.dmp schemas=hr network_link=orcl</i>

<img src="https://user-images.githubusercontent.com/76550825/166072239-7736e65a-3742-4e82-91ff-286baa4ae16d.png" width="427" height="427">

Create HR user in the TSD tablespace and assign the connect and resource roles, as well as privileges to work with the data_pump_dir directory, in order to subsequently import on behalf of the HR user. Grant read – to read the export dump file, write – to write the log file.

<img src="https://user-images.githubusercontent.com/76550825/166072409-bed6ceac-4947-4f56-955d-b983e5299efb.png" width="463" height="136">

Import on behalf of HR with the remap_tablespace parameter to move the MDT database to the TSD tablespace.

<i>impdp hr/hr@odmt directory=data_pump_dir dumpfile=file.dmp tables=hr.employees,hr.departments remap_tablespace=example:tsd</i>

<img src="https://user-images.githubusercontent.com/76550825/166072580-74f53d88-51e9-4d12-9f9b-643686feb4d5.png" width="430" height="280">

Tables moved together with links:

<img src="https://user-images.githubusercontent.com/76550825/166072691-75d126da-bb53-4f6b-94e5-b55595c4ad8a.png" width="624" height="236">

<img src="https://user-images.githubusercontent.com/76550825/166072703-04cfef33-0744-4358-9033-307cddbbc14b.png" width="624" height="140">

<img src="https://user-images.githubusercontent.com/76550825/166072710-d6f45d78-eeff-4451-a189-4f757afafe6e.png" width="624" height="73">



<b>5.</b> Transform the USERS tablespace to a locally managed tablespace:

<img src="https://user-images.githubusercontent.com/76550825/166073023-2a43c1fe-f237-44e2-9ebe-4b4c5bc48eca.png" width="496" height="44">

Let's check that it was successful.

<img src="https://user-images.githubusercontent.com/76550825/166073169-5db254ec-043f-4b67-96f8-65a080bb52c3.png" width="503" height="155">



<b>6.</b> Let's create a locally managed TSL tablespace with a fixed file size (100 M) by moving the DEPARTMENTS table to it.

<img src="https://user-images.githubusercontent.com/76550825/166073274-47cc0655-12c2-43f5-9b0c-fb7e461937a1.png" width="624" height="209">

<img src="https://user-images.githubusercontent.com/76550825/166073284-b8e6a5e4-9dae-4d6d-bf9d-5d7f0a9f8350.png" width="324" height="185">

Thus, the TSL tablespace is created with a fixed file size.

Move the DEPARTMENTS table:

<img src="https://user-images.githubusercontent.com/76550825/166073446-dbb5adad-2121-4090-ab74-18ac3f611067.png" width="622" height="129">

<img src="https://user-images.githubusercontent.com/76550825/166073452-b122b705-397f-4483-b31f-32b5e70b9568.png" width="605" height="63">



<b>7.</b> Let's transform the TSL tablespace into a dictionary-managed tablespace and check:

<img src="https://user-images.githubusercontent.com/76550825/166073628-3965b4c0-e78d-4434-9238-97694aad46d7.png" width="506" height="211">



<b>8.</b> Extent management is automatic, we will get information about the extent sizes in the DEPARTMENTS segment:

<img src="https://user-images.githubusercontent.com/76550825/166073763-81c36cac-d2cc-4035-93b0-144cde67cd10.png" width="592" height="76">



<b>9.</b> Display information about USERS, TSL, and TSL tablespaces:


<img src="https://user-images.githubusercontent.com/76550825/166073887-5cc4a5ba-2b7a-4178-87f0-f13861095bb8.png" width="588" height="105">



<b>10.</b> Let's check the operability of the DEPARTMENTS and EMPLOYEES table by executing a query displaying the names of employees and the names of the departments where they work:

<img src="https://user-images.githubusercontent.com/76550825/166074027-92e2f977-82c1-48d9-a9df-7b46110f185b.png" width="589" height="198">

<img src="https://user-images.githubusercontent.com/76550825/166074086-b36839aa-69df-4cda-88ca-f8391d045dd9.png" width="342" height="156">

In order to avoid the error ORA-00942 table or view does not exist, which occurs as a result of import, it is recommended either to specify the name of the owner of the table, or to make the table public and grant dml privileges, including select, to work with it, or to disable foreign keys constraints, which in our case, according to the condition, cannot be done.

<img src="https://user-images.githubusercontent.com/76550825/166074257-6080fec7-2b76-48c2-b57a-202a0d4367c7.png" width="440" height="287">

<img src="https://user-images.githubusercontent.com/76550825/166074324-e2ab3303-1de5-48e8-8180-1cb12e866f8f.png" width="589" height="199">

<img src="https://user-images.githubusercontent.com/76550825/166074334-0c19b669-de0e-4c34-ba23-7bccfcfc00ff.png" width="421" height="151">

The second option: import with the constraints=y parameter.

<i> impdp hr2/hr2@odmt directory=data_pump_dir dumpfile=file.dmp tables=hr.employees,hr.departments constraints=y remap_tablespace=example:tsd </i>

Then you will not have to make grant public on the tables, which is important for security purposes (for example, it was done for the HR2 scheme).

Export for HR2 is similar to HR (but to a different dump file – file2.dmp):

<img src="https://user-images.githubusercontent.com/76550825/166074582-aff75cd5-0ff5-4278-ba05-772d60d402ff.png" width="427" height="425">

<img src="https://user-images.githubusercontent.com/76550825/166074594-d2030a2a-7460-4da9-b01e-13c0ccf92e18.png" width="428" height="195">

It turns out that you can work with tables on behalf of HR2 without specifying the schema and without the error ORA-00942 (both as sysdba and without specifying):

<img src="https://user-images.githubusercontent.com/76550825/166074732-25acd1a3-be7e-4b70-9d2e-d5e276d8ef68.png" width="589" height="221">

<img src="https://user-images.githubusercontent.com/76550825/166074777-ee4d4648-6dff-4aaf-8592-80eab3cd6d78.png" width="589" height="221">

<img src="https://user-images.githubusercontent.com/76550825/166074785-43f31975-6226-497c-8ca8-001df1e622b2.png" width="407" height="150">
