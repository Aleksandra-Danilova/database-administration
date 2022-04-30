## Problem

Perform a Flashback Transaction Backout of three transactions with WAW dependencies using the LogMiner utility.

## Solution

Since we work with Flashback Transaction, we will perform the following presets, including, we will additionally log information on changing objects and additionally save information on changing the primary key to logs.

<img src="https://user-images.githubusercontent.com/76550825/166094433-43fb289d-570d-4a51-a6c8-440beb7af441.png" width="444" height="187">

Let's execute a transaction with WAW dependencies, that is, when the dependent transaction modifies the same data as the main one.

Let's connect to the HR account and raise the salary of the 100th employee from 24,000 by 3,000 gradually.

<img src="https://user-images.githubusercontent.com/76550825/166094475-345d0ac1-9690-48d9-a026-85a05888f9b8.png" width="264" height="377">

Make a switch of archive logs because Flashback Transaction requires at least one archive redo log to start redo analysis.

<img src="https://user-images.githubusercontent.com/76550825/166094499-048f15ee-58bc-49b0-a218-153c81ee485b.png" width="285" height="72">

Let's make sure that the commands are executed successfully through the Enterprise Manager in the View Data tab.

<img src="https://user-images.githubusercontent.com/76550825/166094509-8e28769a-1575-4bd1-8dea-ee2f2b013046.png" width="480" height="49">

<img src="https://user-images.githubusercontent.com/76550825/166094513-edf4ac5b-53d4-4356-a211-eac989ed245c.png" width="485" height="149">


Open LogMiner in Enterprise Manager->Availability->View and manage transactions. Let's select the EMPLOYEES table.

<img src="https://user-images.githubusercontent.com/76550825/166094534-9df5c682-5307-4bf1-9bb3-dd2a8e695dc5.png" width="531" height="321">

Choose the main transaction, which will be flashed back and cascading dependent ones along with it.

<img src="https://user-images.githubusercontent.com/76550825/166094771-c25b6c36-ca50-4fc9-a890-2d5b95a73445.png" width="531" height="252">

More details:

<img src="https://user-images.githubusercontent.com/76550825/166094782-30b2d352-a0dd-4dc2-81b6-90e7c1bbbe47.png" width="536" height="192">

Click Flashback Transaction:

<img src="https://user-images.githubusercontent.com/76550825/166094792-d780f129-fd18-4419-97fb-9d64c9ccaff6.png" width="532" height="72">

We see dependent transactions:

<img src="https://user-images.githubusercontent.com/76550825/166094798-16ee9542-8839-4ad2-ba75-71a1f51288be.png" width="532" height="189">

Not all of them have [exec=yes], for example:

<img src="https://user-images.githubusercontent.com/76550825/166094806-fb37d4d0-4ef4-4b33-948a-0951e0aa6289.png" width="532" height="249">

Therefore, change the Change Recovery Option to Cascade:

<img src="https://user-images.githubusercontent.com/76550825/166094825-e8da819c-62b4-41a5-b251-cb888cf1f8ef.png" width="535" height="157">

Let's check that there is [exec=yes] everywhere:

<img src="https://user-images.githubusercontent.com/76550825/166094837-169caf4e-b822-4376-a562-1d35f1365fd7.png" width="459" height="207">

<img src="https://user-images.githubusercontent.com/76550825/166094840-8d0b031e-31cf-42eb-8f6e-073152b99d51.png" width="529" height="253">

<img src="https://user-images.githubusercontent.com/76550825/166094842-ce0313d2-5d04-497a-9888-acaab5aa31b7.png" width="460" height="216">

Now you can perform a Flashback Transaction. Let's look at the Undo Sql Script:

<img src="https://user-images.githubusercontent.com/76550825/166094854-8232ef3a-92e3-419b-aba8-c32b818e447f.png" width="429" height="241">

<img src="https://user-images.githubusercontent.com/76550825/166094858-95774626-f209-48ec-beac-af3b27568b22.png" width="433" height="171">

Execute a command in the Execute SQL area to make sure that the changes have been canceled.

<img src="https://user-images.githubusercontent.com/76550825/166094878-17af4b74-dd41-432c-9239-7b08fe75a659.png" width="386" height="133">

Done: transactions have been successfully canceled.

<img src="https://user-images.githubusercontent.com/76550825/166094896-3e2eab95-b52a-4932-998d-de1ef1267c18.png" width="370" height="88">

<img src="https://user-images.githubusercontent.com/76550825/166094898-2280770c-6cf3-44df-93e7-50bc00025953.png" width="225" height="97">
