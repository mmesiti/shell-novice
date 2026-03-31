---
title: File Permissions
teaching: 5
exercises: 5
---

::::::::::::::::::::::::::::::::::::::: objectives

- Share files and directories with other users

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can I tell if I can read and modify a file?

::::::::::::::::::::::::::::::::::::::::::::::::::

On Linux, files and directories have *permissions* 
that decide who can do what with them.

A simple permission mechanism is based on ownership:
each file/directory has a user owner and a group owner. 

To visualize the permission of the content of the current directory, 
we can use `ls` with the `-l` option:

```bash
$ ls -l
total 12
-rw-rw---- 1 michele  testgroup    0 Mär 31 23:16 micheles_file_that_I_can_write
-rw-rw---- 1 training testgroup   29 Mär 31 23:22 my_file_that_michele_can_write
-rw-r----- 1 training testgroup   26 Mär 31 23:22 my_file_that_michele_can_read
-rw--w---- 1 training testgroup    0 Mär 31 23:22 my_file_that_michele_cant_even_read
-rw-rw---- 1 training training     0 Mär 31 23:23 another_file_that_michele_cant_even_read
drwxrwsr-x 2 training testgroup 4096 Mär 31 23:35 a_subdirectory
```
In this directory, there are files belonging to 2 users: `training` and `michele`.
The files also belong to 2 groups: `testgroup` and `training`.

The possible permissions are displayed as the arrays of letters `-rw-r----- `,
which is divided in 4 parts:
- the first position indicates the file type (`-` for regular type, `d` for directory)
- the positions 2-4 represent the permissions for the **user that owns the file**
- the positions 5-7 represent the permissions for the users in the **group that owns the file**
- the positions 8-10 represent the permissions for all other users.


- `r` means that the file is readable 
      (lack of `r` and presence of `-` means no read permission)
- `w` means that the file is writable/modifiable
      (lack of `w` and presence of `-` means no write permission)
- `x` means that the file is executable
      (lack of `x` and presence of `-` means no execute permission)

The user `training` is in the group `training` 
and in the group `testgroup` 
while the user `michele` is only in the group `testgroup`
(and other groups that we are not interested in).
 
The `training` user can verify this with
```bash
$ groups
training testgroup
```

::::::::::::::::::::: challenge

## File Permissions reading comprehension

Given the following output of the `ls -l` command:

```bash
total 12
-rw-rw---- 1 michele  michele      0 Mär 31 23:16 algebra
-rw-r----- 1 michele  testgroup    0 Mär 31 23:16 tomato 
-rw-rw---- 1 michele  testgroup    0 Mär 31 23:16 potato
-rw-rw---- 1 training testgroup   29 Mär 31 23:22 pear 
-rwxr----- 1 training testgroup    0 Mär 31 23:22 car.sh
-rw-rw---- 1 training training     0 Mär 31 23:23 headphones 
```

1. Can `training` read `algebra`?
2. Can `training` remove `tomato`?
3. Can `training` read/wite `potato`?
4. Can `michele` read/wite `pear`?
5. Can `michele` execute `car.sh`?
6. Can `michele` remove `headphones`?


::::::: solution

1. No, the file is owned by user `michele` and group `michele`, and there is no permission for other users
2. `training`  can only read `tomato`, thanks to the fact that `training` is in the `testgroup`, which is the owner group of `tomato`, and the group has only read permissions. So `training` cannote remove/move `tomato`.
3. `training`  can read and write `potato`, thanks to the fact that `training` is in the `testgroup`, which is the owner group of `tomato`.
4. `michele` can read and write `pear` because `michele` is in `testgroup`, which owns the `pear` file and has `rw` permissions on it.
5. No, only `training` can execute `car.sh`, because the group `testgroup` has no execute permissions on it. **BUT:** `michele` can still run it, assuming it is a Bash script, with `bash car.sh`.
6. `michele` cannot do anything on `headphones` because it is owned by `training` and by the `training` group.

::::::::::::::::

:::::::::::::::::::::::::::::::

::::::::::::::::::::::::::: callout
## Changing permissions

There are a few commands that can be used to manipulate permissions:

- **chmod** and **chgrp**: The command `chmod` can be used to change the permissions of files and directories,
and the command `chgrp` can be used to change the group owner of files and directories.
The command `chmod g+s` on a directory owned by `testgroup` 
makes so that by default
files and directories in it are created as belonging to the `testgroup`.

- **umask**: To make sure that all created files are also writable by users of the group, 
the command `umask 0002` should be used in the shell *before the creation*. 
The default value of `umask` is typically `0022`, 
which makes files writable/modifiable only by the user, and not the group.

This can become complicated quickly. 
ACLs might provide a more powerful mechanism to share files and directories.

:::::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::: callout
## More flexibility with ACLs (Access Control Lists)

A more powerful access control mechanism is provided by the ACL mechanism,
that can be used, among other things,
to grant access to files and directories 
to users that are not part of any group we are part of.

To manipulate ACLs, the `getfacl` and `setfacl` commands are used.

:::::::::::::::::::::::::::::::::::


:::::::::::::::::::::::::::::::::::::::: keypoints

- `ls -l` gives information about simple file permissions.
- files and directories have a *user* owner and a *group* owner
- `chown` and `chgrp` can be used to change the permissions of a file
- ACLs give a more powerful mechanism to define ownership
- ACLs can be manipulated with the `getfacl` and `setfacl` commands.

::::::::::::::::::::::::::::::::::::::::::::::::::


