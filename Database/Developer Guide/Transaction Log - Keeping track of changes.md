---
uid: TransactionLogKeepingtrackofchanges
title: Transaction Log - Keeping track of changes
---

SuperOffice has a database-independent synchronization mechanism. It is used by Satellite replication system and by the Travel system.

All changes to tables are recorded in a special transaction log table with timestamps showing when the change happened.

Replication system can then look at what has changed since the previous sync and copy the affected records to the Satellite or Remote Travel system.

All Changes Are Logged
----------------------

When you create/modify/delete a row, the change is logged in the travel transaction log, so that travelers and satellites can be synchronized.

Let’s take a look at what was stored in the log when we create the project “Client SDK Work” .

First we need the project id for the project we created:

SELECT project\_id FROM project WHERE name='Client SDK Work'

Make a note of the id.

Now let us get a list of all the transactions that have happened since midnight this morning:

SELECT \* FROM traveltransactionlog WHERE ttime &gt; '2003.8.7'

(use today's date instead of 2003.8.7)

The date is in YYYY.MM.DD HH:MM:SS format. If you leave out the HH:MM:SS they default to zero (midnight).

The transaction log contains a primary key, the time of the transaction (ttime), who performed the transaction (associate\_id), a type indicating what happened, a table number indicating what table was affected, and the record id of the record in the table that was changed.

![](../Images/TransactionLog.png)

The most common transaction types are:
• New record  = 4352
• Update record = 4608

The table ids are listed in the [reference section](Tables%20by%20number.md).

These are the ones relevant to the new project
• Project table  = 11
• Text table  = 18
• UDProjectSmall  = 142

So we need to scroll down the list of transactions until we find the traces of our new project.

First we see the project being created (table=11, type=4352). Then the text description is added to the project (table=18, type=4352).

You might also see the user-def table having a record inserted (table=142, type=4352)

This table is very useful if you want to monitor updates to the database, or replicate changes to another system. You create a database trigger to replicate changes. SuperOffice does not use triggers or stored-procs because of its database independence. There is nothing to prevent you adding your own, as long as they do not affect SuperOffice’s access to the tables. Adding constraints or throwing exceptions at SuperOffice will cause the CRM client to stop working.

When a traveller returns, all his updates are added to the end of the transaction log, but the timestamps are maintained. The timestamps should be kept in UTC/GMT time - same as the registered and updated fields on the records themselves. This makes conflict resolution easier to handle.

If there are two concurrent updates, then the last update wins. This is rarely a problem in practice, since people tend to "own" a subset of the data that only they modify.

 

Example
![](../Images/ScreenCap3.png)
--------------------------

When the user leaves on travel, his transactions are logged in a separate range of ids. When he returns, the transactions are mapped back into the main sequence.


### See Also:

[traveltransactionlog Table](../Tables/traveltransactionlog.md)