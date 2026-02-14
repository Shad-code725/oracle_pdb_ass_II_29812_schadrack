# Oracle Pluggable Database — Assignment II

**Student Name:** Schadrack Muyisenge NIYOKURI   
**Student ID:** 29812  
**Repository:** oracle_pdb_ass_II_29812_schadrack  
**Course:** Oracle Database / PL-SQL — AUCA  
**Date:** February 14, 2026  

---

## Repository Link
🔗 https://github.com/Shad-code725/oracle_pdb_ass_II_29812_schadrack

---

## Overview

This repository documents the full completion of **Assignment II** for the Oracle Database course at AUCA. The assignment covers the creation and management of Oracle Pluggable Databases (PDBs), including creating a permanent PDB, creating and deleting a temporary PDB, and accessing the Oracle database dashboard using Oracle SQL Developer.

---

## Oracle Environment Used

| Component         | Details                                      |
|-------------------|----------------------------------------------|
| Database Edition  | Oracle AI Database 21c/23ai Free Release         |
| Version           | 23.26.1.0.0                                  |
| Operating System  | Windows 10 Pro (64-bit)                      |
| Data Directory    | C:\APP\KADOSHI\PRODUCT\26AI\ORADATA\FREE\    |
| SQL Client        | SQL*Plus 23.26.1.0.0 + Oracle SQL Developer 24.3.1 |
| Dashboard Tool    | Oracle SQL Developer (used in place of OEM)  |
| Service Name      | FREE                                         |
| Port              | 1521                                         |

---

## Task 1 — Create a Pluggable Database ✅

### Naming Conventions Applied
| Item | Value |
|---|---|
| PDB Name | `sc_pdb_29812` |
| Username inside PDB | `schadrack_plsqlauca_29812` |
| Password | Pass2024 |

### Steps Performed

1. Connected to Oracle 23ai as SYSDBA using SQL*Plus
2. Verified container was set to CDB$ROOT using `SHOW CON_NAME`
3. Set Oracle Managed Files destination using `ALTER SYSTEM SET db_create_file_dest`
4. Created the PDB using `CREATE PLUGGABLE DATABASE sc_pdb_29812 ADMIN USER schadrack_plsqlauca_29812`
5. Opened the PDB using `ALTER PLUGGABLE DATABASE sc_pdb_29812 OPEN`
6. Verified PDB appeared in READ WRITE mode using `SELECT NAME, OPEN_MODE FROM V$PDBS`
7. Switched into the PDB using `ALTER SESSION SET CONTAINER = sc_pdb_29812`
8. Verified user was created using `SELECT USERNAME, ACCOUNT_STATUS FROM DBA_USERS`
9. Saved PDB state using `ALTER PLUGGABLE DATABASE sc_pdb_29812 SAVE STATE`

### Key SQL Commands Used

```sql
-- Set file destination
ALTER SYSTEM SET db_create_file_dest='C:\APP\KADOSHI\PRODUCT\26AI\ORADATA\FREE' SCOPE=BOTH;

-- Create the PDB
CREATE PLUGGABLE DATABASE sc_pdb_29812
  ADMIN USER schadrack_plsqlauca_29812 IDENTIFIED BY Pass2024;

-- Open the PDB
ALTER PLUGGABLE DATABASE sc_pdb_29812 OPEN;

-- Verify PDB is open
SELECT NAME, OPEN_MODE FROM V$PDBS;

-- Switch into PDB
ALTER SESSION SET CONTAINER = sc_pdb_29812;

-- Verify user exists
SELECT USERNAME, ACCOUNT_STATUS FROM DBA_USERS
WHERE USERNAME = 'SCHADRACK_PLSQLAUCA_29812';

-- Save state
ALTER PLUGGABLE DATABASE sc_pdb_29812 SAVE STATE;
```

### Evidence
📸 See `screenshots/task1_pdb_creation.png` — PDB creation command and success message  
📸 See `screenshots/task1_pdb_open.png` — V$PDBS showing SC_PDB_29812 in READ WRITE mode  
📸 See `screenshots/task1_user_created.png` — SCHADRACK_PLSQLAUCA_29812 visible with OPEN status  

---

## Task 2 — Create and Delete a Temporary PDB ✅

### Naming Conventions Applied
| Item | Value |
|---|---|
| Temporary PDB Name | `sc_to_delete_pdb_29812` |

### Steps Performed

1. Returned to CDB$ROOT using `ALTER SESSION SET CONTAINER = CDB$ROOT`
2. Created the temporary PDB using `CREATE PLUGGABLE DATABASE sc_to_delete_pdb_29812`
3. Verified it existed and was open using `SELECT NAME, OPEN_MODE FROM V$PDBS`
4. Closed the PDB using `ALTER PLUGGABLE DATABASE sc_to_delete_pdb_29812 CLOSE IMMEDIATE`
5. Dropped the PDB permanently using `DROP PLUGGABLE DATABASE ... INCLUDING DATAFILES`
6. Confirmed deletion — V$PDBS query returned no row for that PDB

### Key SQL Commands Used

```sql
-- Return to CDB root
ALTER SESSION SET CONTAINER = CDB$ROOT;

-- Create temporary PDB
CREATE PLUGGABLE DATABASE sc_to_delete_pdb_29812
  ADMIN USER admin_temp IDENTIFIED BY Pass2024;

-- Verify it exists
SELECT NAME, OPEN_MODE FROM V$PDBS;

-- Close the PDB
ALTER PLUGGABLE DATABASE sc_to_delete_pdb_29812 CLOSE IMMEDIATE;

-- Drop permanently
DROP PLUGGABLE DATABASE sc_to_delete_pdb_29812 INCLUDING DATAFILES;

-- Confirm deletion
SELECT NAME, OPEN_MODE FROM V$PDBS;
-- Result: SC_TO_DELETE_PDB_29812 no longer appears
```

### Evidence
📸 See `screenshots/task2_temp_pdb_verified.png` — SC_TO_DELETE_PDB_29812 visible in V$PDBS  
📸 See `screenshots/task2_pdb_deleted.png` — V$PDBS after deletion showing PDB is gone  

---

## Task 3 — Oracle Database Dashboard ✅

### Tool Used
Since Oracle 23ai Free edition does not include Oracle Enterprise Manager Express (OEM), **Oracle SQL Developer 24.3.1** was used as the database management dashboard. This tool provides full visibility into the Oracle environment including all PDBs, users, and database status.

### Steps Performed

1. Opened Oracle SQL Developer 24.3.1
2. Created a new connection named `Oracle_23ai_SYSDBA` with:
   - Username: sys
   - Role: SYSDBA
   - Hostname: localhost
   - Port: 1521
   - Service name: FREE
3. Connected successfully to the Oracle 23ai Free database
4. Ran `SELECT NAME, OPEN_MODE FROM V$PDBS` to display all PDBs
5. Confirmed SC_PDB_29812 is visible in READ WRITE mode

### Evidence
📸 See `screenshots/task3_oem_dashboard.png` — SQL Developer dashboard showing SC_PDB_29812 in READ WRITE mode with Oracle_23ai_SYSDBA connection visible  

---

## Screenshots Folder

```
screenshots/
├── task1_pdb_creation.png        — CREATE PLUGGABLE DATABASE command + success message
├── task1_pdb_open.png            — V$PDBS query showing SC_PDB_29812 READ WRITE
├── task1_user_created.png        — SCHADRACK_PLSQLAUCA_29812 with OPEN status
├── task2_temp_pdb_verified.png   — SC_TO_DELETE_PDB_29812 visible in V$PDBS
├── task2_pdb_deleted.png         — V$PDBS after deletion confirming PDB is gone
└── task3_oem_dashboard.png       — SQL Developer dashboard with SC_PDB_29812 visible
```

---

## Challenges Faced & Solutions

| Challenge | Solution Applied |
|---|---|
| `ORA-01017` when connecting as sysdba | Used correct syntax: `sqlplus sys/password@FREE as sysdba` |
| `FILE_NAME_CONVERT` path unknown | Used `SELECT NAME FROM V$DATAFILE` to find Oracle data directory |
| `db_create_file_dest` was empty | Set it manually using `ALTER SYSTEM SET db_create_file_dest=... SCOPE=BOTH` |
| OEM not available on Oracle 23ai Free | Used Oracle SQL Developer 24.3.1 as dashboard alternative |
| SQL Developer required Java | Installed JDK 21 (x64) from Oracle website |
| SQL Developer not appearing in search | Launched directly from Oracle installation directory |

---

## Submission Information

| Field | Value |
|---|---|
| **Repository Link** | https://github.com/Shad-code725/oracle_pdb_ass_II_29812_schadrack |
| **PDB Name Created** | sc_pdb_29812 |
| **Issues Encountered** | Yes (see Challenges section above) |

---

## Integrity Statement

I, **Schadrack** (Student ID: 29812), confirm that all work documented in this repository was performed individually and personally by me. All SQL commands were executed on my own Oracle 23ai Free installation, and all screenshots represent my own work completed on February 14, 2026. This assignment was completed in full accordance with the academic integrity policy of AUCA.

---

*Assignment II — Oracle Pluggable Databases | AUCA | February 2026*
