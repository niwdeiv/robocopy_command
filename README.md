# robocopy_command

ROBOCOPY "x:\source\ (or \\source)" "x:\destination\ (or \\destination)" /MIR /COPYALL /MT:8 /DCOPY:DAT /XF ~* *.TMP /XD DFSRPrivate __DFSR_DIAGNOSTICS_TEST_FOLDER__ /R:1 /W:5 /LOG:C:\robocopy.log



## Robocopy – Pre-Execution Checklist

### Things to consider before running
- **`/MIR` is destructive:** Deletes files/folders in the destination that don't exist in the source. Verify source and destination paths carefully.
- **Permissions:** `/COPYALL` preserves NTFS permissions, ownership, and auditing. Ensure the account has sufficient privileges.
- **DFSR exclusions:** Confirm excluding `DFSRPrivate` and `__DFSR_DIAGNOSTICS_TEST_FOLDER__` is intentional.
- **Open files:** Files actively in use may fail to copy. Consider a maintenance window or final synchronization.
- **Dry run:** Add `/L` to preview operations without copying or deleting anything.
- **Logging:** Review `C:\robocopy.log` for failures and skipped files.

### Switch breakdown

| Switch | Function |
|---|---|
| `/MIR` | Mirrors source to destination; deletes destination-only files/folders. |
| `/COPYALL` | Copies data, attributes, timestamps, NTFS permissions, ownership, and auditing. |
| `/MT:8` | Uses 8 concurrent threads. |
| `/DCOPY:DAT` | Preserves directory data, attributes, and timestamps. |
| `/XF ~* *.TMP` | Excludes files matching these patterns. |
| `/XD DFSRPrivate __DFSR_DIAGNOSTICS_TEST_FOLDER__` | Excludes specified directories. |
| `/XO` | Skips source files older than destination copies. |
| `/R:1` | Retries failed copies once. |
| `/W:5` | Waits 5 seconds between retries. |
| `/LOG:C:\robocopy.log` | Writes results to the specified log file. |

**Important:** If the source is authoritative and the goal is an exact mirror, reconsider `/XO`. 
** It can prevent newer destination files from being overwritten by older source files.
