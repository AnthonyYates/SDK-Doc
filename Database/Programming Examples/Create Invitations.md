---
uid: exampleCreateInvitations
title: Create Invitations
---

Inviting a co-worker, an external person to a meeting in a meeting room.

Here we create four appointment records, one for the inviter, one for the co-worker, one for the external person, and one for the resource.

![](../Images/InvitationDlg.png)

 

Note that the external person (Frantz) and the meeting room (ResourceItem 1) have both automatically accepted the invitation - since they don't have a SuperOffice login.

The co-worker (Ingrid Istad) will get an invitation dialog next time she logs in - and she can then accept the invitation.

In this example the following associate\_ids are useful to know:

| **associate\_id** | **associate type** | **person\_id** | **contact\_id** | **name**             |
|-------------------|--------------------|----------------|-----------------|----------------------|
| 7                 | 0                  | 15             | 2               | Frode Freestad       |
| 10                | 0                  | 18             | 2               | Ingrid Istad         |
| 41                | 1                  | 0              | 0               | Resource item 1      |
| -                 | -                  | 42             | 13              | Frantz Feinschmecker |

 

This is what happens when the SAVE button is clicked:

 

Create invitation appointment for Ingrid Istad
----------------------------------------------

*Note that the appointment points to the master appointment using the mother\_id field.*

*The invitedPersonId field is set to the associate's person\_id.*

```SQL
INSERT INTO CRM5."appointment" ("appointment\_id", "contact\_id", "person\_id", "**associate\_id**", "group\_idx", "registered", "registered\_associate\_id", "done", "do\_by", "leadtime", "task\_idx", "priority\_idx", "type", "status", "private", "alarm", "text\_id", "project\_id", "**mother\_id**", "document\_id", "color\_index", "opportunity\_id", "**invitedPersonId**", "activeDate", "endDate", "lagTime", "source", "userdef\_id", "userdef2\_id", "updated", "updated\_associate\_id", "updatedCount", "activeLinks", "recurrenceRuleId", "location", "alldayEvent", "freeBusy", "rejectCounter", "emailId", "rejectReason", "hasAlarm", "assignedBy") VALUES (740, 13, 42, **10**, 5, 1164896595, 7, 0, 1164888900, 0, 8, 0, 6, 5, 0, 0, 389, 0, **739**, 0, 0, 0, **18**, 1164888900, 1164894300, 0, 0, 0, 0, 0, 0, 0, 0, 0, '', 0, 0, 0, 0, '', 0, 0)

INSERT INTO CRM5."traveltransactionlog" ("traveltransactionlog\_id", "ttime", "prev\_record\_id", "type", "associate\_id", "tablenumber", "record\_id") VALUES (110444, 1164900195, 0, 4352, 7, 9, 740)

INSERT INTO CRM5."visiblefor" ("VisibleFor\_id", "tableId", "recordId", "forAll", "forGroupId", "forAssocId", "encryptedCheck", "registered", "registered\_associate\_id", "updated", "updated\_associate\_id", "updatedCount") VALUES (915, 9, 740, 1, 0, 0, 'TU6UOTaVZLKiFhgQyTEIuYatA57qbFEo', 1164896595, 7, 0, 0, 0)

INSERT INTO CRM5."traveltransactionlog" ("traveltransactionlog\_id", "ttime", "prev\_record\_id", "type", "associate\_id", "tablenumber", "record\_id") VALUES (110445, 1164900195, 0, 4352, 7, 196, 915)
```
 

Create invitation appointment for Frantz Feinsmecher
----------------------------------------------------

*Note that associate\_id = 0 since Frantz is not a user, but invited\_person\_id = Frantz*

```SQL
INSERT INTO CRM5."appointment" ("appointment\_id", "contact\_id", "person\_id", "**associate\_id**", "group\_idx", "registered", "registered\_associate\_id", "done", "do\_by", "leadtime", "task\_idx", "priority\_idx", "type", "status", "private", "alarm", "text\_id", "project\_id", "**mother\_id**", "document\_id", "color\_index", "opportunity\_id", "**invitedPersonId**", "activeDate", "endDate", "lagTime", "source", "userdef\_id", "userdef2\_id", "updated", "updated\_associate\_id", "updatedCount", "activeLinks", "recurrenceRuleId", "location", "alldayEvent", "freeBusy", "rejectCounter", "emailId", "rejectReason", "hasAlarm", "assignedBy") VALUES (741, 13, 42, **0**, 0, 1164896595, 7, 0, 1164888900, 0, 8, 0, 1, 1, 0, 0, 389, 0, **739**, 0, 0, 0, **42**, 1164888900, 1164894300, 0, 0, 0, 0, 0, 0, 0, 0, 0, '', 0, 0, 0, 0, '', 0, 0)

INSERT INTO CRM5."traveltransactionlog" ("traveltransactionlog\_id", "ttime", "prev\_record\_id", "type", "associate\_id", "tablenumber", "record\_id") VALUES (110446, 1164900195, 0, 4352, 7, 9, 741)

INSERT INTO CRM5."visiblefor" ("VisibleFor\_id", "tableId", "recordId", "forAll", "forGroupId", "forAssocId", "encryptedCheck", "registered", "registered\_associate\_id", "updated", "updated\_associate\_id", "updatedCount") VALUES (916, 9, 741, 1, 0, 0, 'xt5SMR9bL92iFhgQyTEIuYatA57qbFEo', 1164896595, 7, 0, 0, 0)

INSERT INTO CRM5."traveltransactionlog" ("traveltransactionlog\_id", "ttime", "prev\_record\_id", "type", "associate\_id", "tablenumber", "record\_id") VALUES (110447, 1164900195, 0, 4352, 7, 196, 916)
```
 

Create invitation appointment for resource item 1
-------------------------------------------------

*The resource is an associate with no person, sohere the associate\_id = 41, but the invitedPerson\_id = 0*

```SQL
INSERT INTO CRM5."appointment" ("appointment\_id", "contact\_id", "person\_id", "**associate\_id**", "group\_idx", "registered", "registered\_associate\_id", "done", "do\_by", "leadtime", "task\_idx", "priority\_idx", "type", "status", "private", "alarm", "text\_id", "project\_id", "**mother\_id**", "document\_id", "color\_index", "opportunity\_id", "**invitedPersonId**", "activeDate", "endDate", "lagTime", "source", "userdef\_id", "userdef2\_id", "updated", "updated\_associate\_id", "updatedCount", "activeLinks", "recurrenceRuleId", "location", "alldayEvent", "freeBusy", "rejectCounter", "emailId", "rejectReason", "hasAlarm", "assignedBy") VALUES (742, 13, 42, **41**, 4, 1164896595, 7, 0, 1164888900, 0, 8, 0, 1, 1, 0, 0, 389, 0, **739**, 0, 0, 0, **0**, 1164888900, 1164894300, 0, 0, 0, 0, 0, 0, 0, 0, 0, '', 0, 0, 0, 0, '', 0, 0)

INSERT INTO CRM5."traveltransactionlog" ("traveltransactionlog\_id", "ttime", "prev\_record\_id", "type", "associate\_id", "tablenumber", "record\_id") VALUES (110448, 1164900195, 0, 4352, 7, 9, 742)

INSERT INTO CRM5."visiblefor" ("VisibleFor\_id", "tableId", "recordId", "forAll", "forGroupId", "forAssocId", "encryptedCheck", "registered", "registered\_associate\_id", "updated", "updated\_associate\_id", "updatedCount") VALUES (917, 9, 742, 1, 0, 0, '0/BOOig87Y+iFhgQyTEIuYatA57qbFEo', 1164896595, 7, 0, 0, 0)

INSERT INTO CRM5."traveltransactionlog" ("traveltransactionlog\_id", "ttime", "prev\_record\_id", "type", "associate\_id", "tablenumber", "record\_id") VALUES (110449, 1164900195, 0, 4352, 7, 196, 917)
```
 

Create master appointment for Frode Freestad
--------------------------------------------

*The master appointment is recognizable by the appointment\_id = mother\_id*

```SQL
INSERT INTO CRM5."appointment" ("appointment\_id", "contact\_id", "person\_id", "associate\_id", "group\_idx", "registered", "registered\_associate\_id", "done", "do\_by", "leadtime", "task\_idx", "priority\_idx", "type", "status", "private", "alarm", "text\_id", "project\_id", "mother\_id", "document\_id", "color\_index", "opportunity\_id", "invitedPersonId", "activeDate", "endDate", "lagTime", "source", "userdef\_id", "userdef2\_id", "updated", "updated\_associate\_id", "updatedCount", "activeLinks", "recurrenceRuleId", "location", "alldayEvent", "freeBusy", "rejectCounter", "emailId", "rejectReason", "hasAlarm", "assignedBy") VALUES (739, 13, 42, 7, 4, 1164896595, 7, 0, 1164888900, 0, 8, 0, 1, 1, 0, 0, 389, 0, 739, 0, 0, 0, 15, 1164888900, 1164894300, 0, 0, 0, 0, 0, 0, 0, 0, 0, '', 0, 0, 0, 0, '', 0, 0)

INSERT INTO CRM5."traveltransactionlog" ("traveltransactionlog\_id", "ttime", "prev\_record\_id", "type", "associate\_id", "tablenumber", "record\_id") VALUES (110450, 1164900195, 0, 4352, 7, 9, 739)

INSERT INTO CRM5."visiblefor" ("VisibleFor\_id", "tableId", "recordId", "forAll", "forGroupId", "forAssocId", "encryptedCheck", "registered", "registered\_associate\_id", "updated", "updated\_associate\_id", "updatedCount") VALUES (918, 9, 739, 1, 0, 0, 'BDTWlNDVe6qiFhgQyTEIuYatA57qbFEo', 1164896596, 7, 0, 0, 0)

INSERT INTO CRM5."traveltransactionlog" ("traveltransactionlog\_id", "ttime", "prev\_record\_id", "type", "associate\_id", "tablenumber", "record\_id") VALUES (110452, 1164900196, 0, 4352, 7, 196, 918)
```

*Note that the same text record is shared between all the appointments*

```SQL
INSERT INTO CRM5."text" ("text\_id", "type", "owner\_id", "registered", "registered\_associate\_id", "updated", "updated\_associate\_id", "updatedCount", "text", "lcid", "seqno") VALUES (389, 4, 739, 1164896596, 7, 0, 0, 0, 'Lunch meeting', 1044, 0)

INSERT INTO CRM5."traveltransactionlog" ("traveltransactionlog\_id", "ttime", "prev\_record\_id", "type", "associate\_id", "tablenumber", "record\_id") VALUES (110451, 1164900196, 0, 4352, 7, 18, 389)
```

*Now we add the words in the text record to the freetext index*

```SQL
DELETE FROM CRM5."freetextindex" WHERE "table\_id" = 18 AND "record\_id" = 389

INSERT INTO CRM5."freetextwords" ("freetextwords\_id", "word") VALUES (2086811941, 'LUNCH')

INSERT INTO CRM5."freetextwords" ("freetextwords\_id", "word") VALUES (535965901, 'MEETING')

INSERT INTO CRM5."freetextindex" ("freetextindex\_id", "freetextwords\_id", "table\_id", "record\_id", "ownertable\_id", "ownerrecord\_id", "infile") VALUES (790392919, 2086811941, 18, 389, 9, 739, 0)

INSERT INTO CRM5."freetextindex" ("freetextindex\_id", "freetextwords\_id", "table\_id", "record\_id", "ownertable\_id", "ownerrecord\_id", "infile") VALUES (1431706010, 535965901, 18, 389, 9, 739, 0)
```
 

Update SAINT counters for "Fritz & Frantz Import"
-------------------------------------------------

```SQL
UPDATE CRM5."countervalue" SET "CounterValue\_id" = 16402, "contact\_id" = 13, "person\_id" = 0, "project\_id" = 0, "extra1\_id" = 0, "extra2\_id" = 0, "record\_type" = 1, "direction" = 3, "intent\_id" = 0, "sale\_status" = 0, "amountClassId" = 0, "totalReg" = 1, "totalRegInPeriod" = 1, "notCompleted" = 1, "notCompletedInPeriod" = 1, "lastRegistered" = 1164888900, "lastCompleted" = 0, "lastDoBy" = 1164888900, "extra1\_count" = 0, "extra2\_count" = 0, "extra3\_count" = 0, "extra4\_count" = 0, "registered" = 0, "registered\_associate\_id" = 0, "updated" = 1164896596, "updated\_associate\_id" = 7, "updatedCount" = 0 WHERE "CounterValue\_id" = 16402

UPDATE CRM5."countervalue" SET "CounterValue\_id" = 16406, "contact\_id" = 13, "person\_id" = 0, "project\_id" = 0, "extra1\_id" = 0, "extra2\_id" = 0, "record\_type" = 1, "direction" = 3, "intent\_id" = 5, "sale\_status" = 0, "amountClassId" = 0, "totalReg" = 1, "totalRegInPeriod" = 1, "notCompleted" = 1, "notCompletedInPeriod" = 1, "lastRegistered" = 1164888900, "lastCompleted" = 0, "lastDoBy" = 1164888900, "extra1\_count" = 0, "extra2\_count" = 0, "extra3\_count" = 0, "extra4\_count" = 0, "registered" = 0, "registered\_associate\_id" = 0, "updated" = 1164896596, "updated\_associate\_id" = 7, "updatedCount" = 0 WHERE "CounterValue\_id" = 16406

UPDATE CRM5."countervalue" SET "CounterValue\_id" = 16591, "contact\_id" = 13, "person\_id" = 0, "project\_id" = 0, "extra1\_id" = 0, "extra2\_id" = 0, "record\_type" = 10, "direction" = 3, "intent\_id" = 0, "sale\_status" = 0, "amountClassId" = 0, "totalReg" = 1, "totalRegInPeriod" = 1, "notCompleted" = 1, "notCompletedInPeriod" = 1, "lastRegistered" = 1164888900, "lastCompleted" = 0, "lastDoBy" = 1164888900, "extra1\_count" = 0, "extra2\_count" = 0, "extra3\_count" = 0, "extra4\_count" = 0, "registered" = 0, "registered\_associate\_id" = 0, "updated" = 1164896596, "updated\_associate\_id" = 7, "updatedCount" = 0 WHERE "CounterValue\_id" = 16591

UPDATE CRM5."countervalue" SET "CounterValue\_id" = 16595, "contact\_id" = 13, "person\_id" = 0, "project\_id" = 0, "extra1\_id" = 0, "extra2\_id" = 0, "record\_type" = 10, "direction" = 3, "intent\_id" = 5, "sale\_status" = 0, "amountClassId" = 0, "totalReg" = 1, "totalRegInPeriod" = 1, "notCompleted" = 1, "notCompletedInPeriod" = 1, "lastRegistered" = 1164888900, "lastCompleted" = 0, "lastDoBy" = 1164888900, "extra1\_count" = 0, "extra2\_count" = 0, "extra3\_count" = 0, "extra4\_count" = 0, "registered" = 0, "registered\_associate\_id" = 0, "updated" = 1164896596, "updated\_associate\_id" = 7, "updatedCount" = 0 WHERE "CounterValue\_id" = 16595
```
 


### See Also:

[Appointment Table](../Tables/appointment.md)