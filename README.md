# Oracle Pluggable Database — Assignment II

**Student Name:** Schadrack Muyisenge NIYOKURI
**Student ID:** 29812
**Repository:** oracle_pdb_ass_II_29812_schadrack
**Course:** Oracle Database / PL-SQL AUCA
**Date:** February 2026

---

## Overview

This repository documents the completion of **Assignment II** for the Oracle Database course at AUCA. The assignment covers the creation, management, and deletion of Oracle Pluggable Databases (PDBs), as well as configuration and access of Oracle Enterprise Manager (OEM).

### Oracle Environment Used

| Component         | Details                            |
|-------------------|------------------------------------|
| Database Type     | Oracle Database 19c / 21c (CDB)   |
| Container DB      | ORCL (CDB$ROOT)                    |
| Operating System  | Oracle Linux / Windows Server      |
| OEM Port          | 5500 (HTTPS)                       |
| SQL Client        | SQL*Plus / SQL Developer           |

---

## Tasks Completed

### Task 1 — Create a Pluggable Database

A new Pluggable Database was created following the exact naming conventions provided.

**PDB Name:** `sc_pdb_29812`
**Username inside PDB:** `schadrack_plsqlauca_29812`

**Steps performed:**
1. Connected to Oracle as SYSDBA
2. Created the PDB using `CREATE PLUGGABLE DATABASE` with `FILE_NAME_CONVERT`
3. Opened the PDB using `ALTER PLUGGABLE DATABASE sc_pdb_29812 OPEN`
4. Switched container session to the new PDB
5. Granted `CONNECT`, `RESOURCE`, and `DBA` privileges to the user
6. Verified the PDB and user exist using `V$PDBS` and `DBA_USERS`
7. Saved the PDB state for auto-open on restart

**Key SQL Used:**
```sql
CREATE PLUGGABLE DATABASE sc_pdb_29812
  ADMIN USER schadrack_plsqlauca_29812 IDENTIFIED BY Pass2024
  FILE_NAME_CONVERT = ('/opt/oracle/oradata/ORCL/pdbseed/',
                       '/opt/oracle/oradata/ORCL/sc_pdb_29812/');

ALTER PLUGGABLE DATABASE sc_pdb_29812 OPEN;
SELECT NAME, OPEN_MODE FROM V$PDBS;
```

📸 *See screenshots: `screenshots/task1_pdb_creation.png`, `screenshots/task1_pdb_open.png`, `screenshots/task1_user_created.png`*

---

### Task 2 — Create and Delete a Temporary PDB

A temporary PDB was created, verified, and then permanently deleted to demonstrate full PDB lifecycle management.

**Temporary PDB Name:** `sc_to_delete_pdb_29812`

**Steps performed:**
1. Created the temporary PDB from CDB$ROOT
2. Opened and verified the PDB exists using `V$PDBS`
3. Closed the PDB with `CLOSE IMMEDIATE`
4. Dropped the PDB permanently using `DROP PLUGGABLE DATABASE ... INCLUDING DATAFILES`
5. Confirmed deletion — query returned `no rows selected`

**Key SQL Used:**
```sql
CREATE PLUGGABLE DATABASE sc_to_delete_pdb_29812
  ADMIN USER admin_temp IDENTIFIED BY Pass2024
  FILE_NAME_CONVERT = ('/opt/oracle/oradata/ORCL/pdbseed/',
                       '/opt/oracle/oradata/ORCL/sc_to_delete_pdb_29812/');

ALTER PLUGGABLE DATABASE sc_to_delete_pdb_29812 CLOSE IMMEDIATE;
DROP PLUGGABLE DATABASE sc_to_delete_pdb_29812 INCLUDING DATAFILES;

SELECT NAME, OPEN_MODE FROM V$PDBS
WHERE NAME = 'SC_TO_DELETE_PDB_29812';
-- Result: no rows selected
```

📸 *See screenshots: `screenshots/task2_temp_pdb_created.png`, `screenshots/task2_pdb_deleted.png`, `screenshots/task2_deletion_confirmed.png`*

---

### Task 3 — Oracle Enterprise Manager (OEM)

Oracle Enterprise Manager was accessed via the browser at `https://localhost:5500/em`.

The OEM dashboard was used to:
- Verify the Oracle CDB environment
- Confirm `sc_pdb_29812` appears in the Pluggable Databases list with **READ WRITE** status
- View performance metrics and database health

📸 *See screenshot: `screenshots/task3_oem_dashboard.png`*

---

## Screenshots

All screenshots are located in the `screenshots/` folder:

```
screenshots/
├── task1_pdb_creation.png       — CREATE PLUGGABLE DATABASE command output
├── task1_pdb_open.png           — PDB in READ WRITE open state (V$PDBS query)
├── task1_user_created.png       — User schadrack_plsqlauca_29812 visible in DBA_USERS
├── task2_temp_pdb_created.png   — Temporary PDB created successfully
├── task2_pdb_deleted.png        — DROP command executed
├── task2_deletion_confirmed.png — V$PDBS query showing no rows selected
└── task3_oem_dashboard.png      — OEM dashboard with PDB and username visible
```

---

## Challenges Faced

| Challenge | Solution |
|-----------|----------|
| FILE_NAME_CONVERT path mismatch | Verified correct Oracle data directory using `SHOW PARAMETER db_create_file_dest` |
| PDB not auto-opening after restart | Used `ALTER PLUGGABLE DATABASE sc_pdb_29812 SAVE STATE` |
| OEM port not accessible | Ran `EXEC DBMS_XDB_CONFIG.SETHTTPSPORT(5500)` and checked firewall rules |

---

## Submission Information

| Field | Value |
|-------|-------|
| Repository Link | https://github.com/schadrack/oracle_pdb_ass_II_29812_schadrack |
| PDB Name Created | sc_pdb_29812 |
| Issues Encountered | Yes (see Challenges section) |

---

## Integrity Statement

I, **Schadrack** (Student ID: 29812), confirm that all work documented in this repository was performed individually by me. All SQL commands were executed personally in my Oracle environment, and all screenshots represent my own work. This assignment was completed in accordance with the academic integrity policy of AUCA.

---

*Assignment II — Oracle Pluggable Databases | AUCA | February 2026*
