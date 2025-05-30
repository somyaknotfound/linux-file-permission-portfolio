# File Permissions in Linux - Technical Project Report

## Project Description

The research team at my organization needs to update the file permissions for certain files and directories within the projects directory. The permissions do not currently reflect the level of authorization that should be given. Checking and updating these permissions will help keep their system secure. 

To complete this task, I performed the following tasks:

## Check File and Directory Details

The following demonstrates how I used Linux commands to determine the existing permissions set for a specific directory in the file system.

![Initial Directory Listing](./screenshots/permission-changes.png)

The first line shows the list of directories under the current parent folder and it shows our projects folder on which we need to work. After that we navigate to the projects directory and check permissions for all contents inside it using `ls -la` command. 

**Key Findings:**
- The file content returned had 3 different users who had various access over different files and directories in the project directory
- There was a hidden file `.project_x.txt` 
- Five other project files were present
- The 10-character string in the first column represents the permissions set on each file or directory

## Describe the Permissions String

The 10-character string can be deconstructed to determine who is authorized to access the file and their specific permissions. The characters and what they represent are as follows:

### Permission String Breakdown

| Position | Description | Details |
|----------|-------------|---------|
| **1st character** | File type indicator | `d` = directory, `-` = regular file |
| **2nd-4th characters** | User permissions | Read (r), Write (w), Execute (x) for the user |
| **5th-7th characters** | Group permissions | Read (r), Write (w), Execute (x) for the group |
| **8th-10th characters** | Other permissions | Read (r), Write (w), Execute (x) for other users |

When any of these characters is a hyphen (-) instead, it indicates that the specific permission is not granted.

### Example Analysis

For example, the file permissions for `project_t.txt` are `-rw-rw-r--`:

- **1st character (`-`)**: Indicates `project_t.txt` is a file, not a directory
- **2nd, 5th, and 8th characters (`r`)**: User, group, and other all have read permissions
- **3rd and 6th characters (`w`)**: Only the user and group have write permissions
- **Execute permissions**: No one has execute permissions for `project_t.txt`

## Change File Permissions

As per the organization's norms, others should not have access to write on any files, so we remove the write permission for all the other owners in the project files.

![Permission Changes](./screenshots/permission-changes2.png)

### Commands Executed:
```bash
chmod o-w project_k.txt
chmod o-w project_m.txt  
chmod o-w project_r.txt
chmod o-w project_t.txt
```

**Result:** No other owners have write permissions to content in the projects directory.

## Change File Permissions on a Hidden File

The hidden file should not have write permissions for anyone, but the user and group should be able to read the file. For this I ran the following commands:

![Hidden File Permissions](./screenshots/permission-changes1.png)

### Commands Executed:
```bash
chmod u-w,g-w,g+r .project_x.txt
```

**Breakdown:**
- `u-w`: Remove write permission for user
- `g-w`: Remove write permission for group  
- `g+r`: Add read permission for group

## Change Directory Permissions

The directory should only have executable access to researcher2. As we can see, the drafts directory within the projects directory has execute permissions that need to be modified so that only researcher2 has the permission.

![Directory Permissions](./screenshots/final-verification.png)

### Commands Executed:
```bash
chmod g-x drafts
```

### Results Analysis:
The output displays the permission listing for several files and directories:

1. **Line 1**: Current directory (projects)
2. **Line 2**: Parent directory (home)  
3. **Line 3**: Regular file titled `.project_x.txt`
4. **Line 4**: Directory (drafts) with restricted permissions

**Key Achievement:** Only researcher2 now has execute permissions on the drafts directory. The group previously had execute permissions, so I used the chmod command to remove them. The researcher2 user already had execute permissions, so they did not need to be added.

## Security Impact Assessment

### Before Implementation:
- Multiple users had inappropriate access levels
- "Other" users had write permissions to sensitive files
- Hidden files were not properly secured
- Directory permissions allowed broader access than necessary

### After Implementation:
- Implemented principle of least privilege
- Removed write access for "other" users across all project files
- Secured hidden files with appropriate read-only access
- Restricted directory execution to authorized user only

## Technical Commands Summary

| Command | Purpose | Files Affected |
|---------|---------|----------------|
| `ls -la` | List detailed file permissions | All files in projects directory |
| `chmod o-w` | Remove write permission for others | project_k.txt, project_m.txt, project_r.txt, project_t.txt |
| `chmod u-w,g-w,g+r` | Modify hidden file permissions | .project_x.txt |
| `chmod g-x` | Remove group execute permission | drafts directory |

## Summary

I successfully changed multiple permissions to match the level of authorization my organization wanted for files and directories in the projects directory. The methodology followed was:

1. **Assessment Phase**: Used `ls -la` to check current permissions for the directory
2. **Analysis Phase**: Identified security gaps and inappropriate access levels
3. **Implementation Phase**: Used `chmod` command multiple times to adjust permissions
4. **Verification Phase**: Confirmed changes met organizational security requirements

This systematic approach ensured that the file permission updates enhanced system security while maintaining necessary access for authorized users.

## Compliance and Best Practices

The implemented changes align with cybersecurity best practices:
- **Principle of Least Privilege**: Users have minimum necessary permissions
- **Access Control**: Clear separation between user, group, and other permissions
- **Security Hardening**: Removed unnecessary write access to prevent unauthorized modifications
- **Audit Trail**: All changes documented and verifiable

This project demonstrates effective Linux system administration skills with a focus on security implementation and access control management.
