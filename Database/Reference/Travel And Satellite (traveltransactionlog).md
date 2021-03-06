---
uid: TravelAndSatellite(traveltransactionlog)
title: Travel And Satellite (traveltransactionlog)
---

---
uid: infoTraveltransactionlog
title: Travel And Satellite (traveltransactionlog)
---

Note about using Travel function or Corporate version
=====================================================

SuperOffice makes it possible to update databases in different locations using transaction logs. This functionality is sold under the names Travel, Satellite and Area Management.

The logs are updated when a SuperOffice user changes data. They will not be automatically updated by the database when inserting, updating or deleting from outside SuperOffice. NOTE! They are updated when data manipulation is done through SuperCOM, SuperOffice OleDB provider or NetServer to SuperOffice. We therefore recommend that you use our tools to make updates to the database.

**Purpose of the log
**The table traveltransactionlog (crm7.traveltransactionlog in ODBC databases, also referred to as the "log") is used to keep track of all updates, that is, insertions, deletions and changes to all data records in SuperOffice.  It is used by the update functions in Travel (local update, async update, central update) and Satellite (up, down files) to determine what to send.

The log contains one record for each change.  The record does not actually contain the data that was changed, only a reference to the table and record id of the changed record.

**Prefix on travel**

Below all tables in the database manual, you will find e.g. “Prefix on travel: 0x0000007e”. This is the ID that SuperOffice CRM 5 adds to new records on travel. Take the last byte, and move it 24 bits to the left, and you have the number added to the allocated next\_id from sequence during travel. It’s restored to normal low ID’s when the traveller performs a homecoming.

**Format**
The record definition looks like this:

|                  |              |                 |                          |
|------------------|--------------|-----------------|--------------------------|
| **C++ DataType** | **C++ Name** | **DB DataType** | **DB Name**              |
| Longid           | id           | int             | traveltransactionlog\_id |
| date\_t          | time         | int             | ttime                    |
| longid           | mode         | int             | prev\_record\_id         |
| Ushort           | type         | smallint        | type                     |
| LongId           | assoc\_id    | int             | associate\_id            |
| Ushort           | tabno        | smallint        | tablenumber              |
| LongId           | rec\_id      | int             | record\_id               |

Use of fields
-------------

*id
*This field identifies the record in the traveltransactionlog (not the record changed).  Its value is taken from sequence, using sequence row 39.  The standard SQL statement used to get new id's is:

*BEGIN TRANSACTION
UPDATE so.sequence SET next\_id = next\_id + 1 WHERE id = 39
SELECT next\_id - 1 FROM so.sequence WHERE id = 39
COMMIT TRANSACTION*

If an application requires new ids for several tables it will be able to group all the update statements inside a single begin/commit transaction block; it is also legal to increment a sequence row by more than 1 if you need more than 1 new row in one table.  Do not change the order to select/update as this would not be multi-user safe.  Note also that the actual new id is not the next\_id in the table, but next\_id - 1.

*ttime*
This is a standard SuperOffice date/time value, i.e., number of seconds since 1.1.1970 00:00.  The PC's local clock is used, which may introduce some inaccuracies in the update logic if two users make near-simultaneous updates to the same record and their PCs do not have synchronised clocks.  Ideally, the PC clock should be synchronised with an external source when using Travel functions.  The time field is a timestamp that shows when the record update (or insertion or deletion) was done (i.e. when the traveltransactionlog record was created).

*Prev\_record\_id*
This field is now used for additional information. It is normally set to “0”, except in these situations:
Type = 5120 kTrtRecUpdateOwner (see below).  In that case, the mode field contains the previous owner id. 
The owner id is an associate id that contains the owner of a record; it refers to these tables and fields:

|             |               |
|-------------|---------------|
| **Table**   | **Field**     |
| Contact     | associate\_id |
| Project     | associate\_id |
| Appointment | associate\_id |
| Sale        | associate\_id |

The logic is:

prepare and write normal traveltransactionlog record

*if operation = update
   if associate\_id is changed and table in (contact, project, appointment, sale)
         mode = previous associate\_id
         type = kTrtRecUpdateOwner
         set id, time, tabno, rec\_id
         write traveltransactionlog    end
end*

 

This functionality is only relevant if you are using Area Management. Area Management uses the owner associate id as one of the criteria for determining which area a record belongs to.  If the owner id is changed it might trigger the transfer of that record from one area (satellite) to another, translating an update operation into a delete/insert pair on separate areas.

The extra traveltransactionlog record contains the previous owner id (which is not available anywhere else) so that the area management system can determine what to do.

**Type = 5888.** User (Associate\_id) removed from Area user inclusion

**Type = 6144.** User (Associate\_id) deleted from Area user assignment

**Type = 6400.** Prev\_record\_id= version of user defined fields that’s been published (udeffield.version).
Tablenumber: contact = 7, person = 8, project = 9, sale = 10.

**Type = 8192 -&gt; 8200**. Used with Field level replication. It’s a bit mask of which fields in the record that has been changed, 1 means fields been changed. This means if the second field in a record has been changed, then pref\_record\_id=2.

Value for field type in traveltransctionlog
===========================================

This is the type of update the log record refers to.  Valid types are:

|                          |        |                                                                                                              |
|--------------------------|--------|--------------------------------------------------------------------------------------------------------------|
| **type**                 | **id** | **Comment**                                                                                                  |
| Unknown                  | 0      | Unknown - used when initializing                                                                             |
| Insert                   | 4352   | A record has been inserted                                                                                   |
| Update                   | 4608   | A record has been updated                                                                                    |
| Delete                   | 4864   | A record has been deleted                                                                                    |
| UpdateOwner              | 5120   | The owning associate\_id of the target record has been updated, old value in PrevrecordId                    |
| OldUpdateContact         | 5376   | (Obsolete) The contact\_id of the target record has been updated, old value in PrevRecordId                  |
| OldUpdateProject         | 5632   | (Obsolete) The project\_id of the target record has been updated, old value in PrevRecordId                  |
| DeleteAreaUserInclusion  | 5888   | A previously included (data inclusion) user has been removed from an area                                    |
| DeleteAreaUserAssignment | 6144   | A previously assigned (login rights) user has been delete from an Area                                       |
| PublishUdef              | 6400   | A new layout for User-defined fields has been published (tablenumber = 7, 8, 9 or 10 (check table udeffield) |
| UpdateFieldPart1         | 8192   | (Reserved) Bitmap of updated fields                                                                          |
| UpdateFieldPart2         | 8193   | (Reserved) Bitmap of updated fields                                                                          |
| UpdateFieldPart3         | 8194   | (Reserved) Bitmap of updated fields                                                                          |
| UpdateFieldPart4         | 8195   | (Reserved) Bitmap of updated fields                                                                          |
| UpdateFieldPart5         | 8196   | (Reserved) Bitmap of updated fields                                                                          |
| UpdateFieldPart6         | 8197   | (Reserved) Bitmap of updated fields                                                                          |
| UpdateFieldPart7         | 8198   | (Reserved) Bitmap of updated fields                                                                          |
| UpdateFieldPart8         | 8199   | (Reserved) Bitmap of updated fields                                                                          |
| UpdateContact            | 10000  | The contact\_id of the target record has been changed, old value in PrevRecordId                             |
| UpdateProject            | 20000  | The project\_id of the target record has been updated, odl value in PrevRecordId                             |

 

An updateowner is always **preceded** by the corresponding update record.

*associate\_id*
The associate\_id of the user actually performing the update (not an associate\_id from the record being updated!).

*tablenumber*
The table id of the table being updated.  Show all tables will give you these values.

*record\_id*
The record id of the record being updated.  tabno and rec\_id together uniquely identify the record that is being updated, and the synchronisation/replication mechanism in SuperOffice can use these ids to look up the actual record values.

Comments
--------

Any updates to the database that are not logged in traveltransactionlog will not be replicated to/from travel users and between satellites.  An application should generally either log all updates or none.  If a record in SuperOffice refers to other records (often the case), and not all of the records involved are logged, travel users may end up with an inconsistent database.  Not recommended.