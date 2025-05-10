
# File Permissions

On a Linux system, each file and directory is assigned access rights for the owner of the file, the members of a group of related users, and everybody else. Rights can be assigned to read a file, to write a file, and to execute a file (i.e., run the file as a program).

* chmod - change mode - modify file access rights
* chown - change file ownership
* chgrp - change a file's group ownership

Examples: ```-rwxrwxrwx```, ```drwxrwxrwx```

```-``` at the beginning of the permission string indicates a file, ```d``` indicates a directory

Followed by 3 triplets of permissions for: ```owner | group | others```
where ```rwx = read, write, execute```

## Octal notation

```
rwx rwx rwx = 111 111 111
rw- rw- rw- = 110 110 110
rwx --- --- = 111 000 000
```

```r = 4, w = 2, x = 1```

```
rwx = 111 in binary = 7
rw- = 110 in binary = 6
r-x = 101 in binary = 5
r-- = 100 in binary = 4
```

Therefore, we can represent the permissions as a 3-digit octal number.

## chmod

```bash
chmod 755 file.txt
```

```755``` means the owner has read, write, and execute permissions, and the group and others have read and execute permissions.

```chmod``` can also be used for directories, most useful permission sets:

```
777 -- no restrictions on permissions
755 -- owner has full access, group and others can only read and execute, cannot create / delete files
700 -- only owner has full access
```


```
x - Allows a directory to be entered (cd dir...).
r - Allows the contents of the directory to be listed (x is set).
w - Allows files within the directory to be created, deleted, or renamed (x is set).
```

## chown
we must have superuser privileges to change the owner of a file.

```bash
sudo chown user_group file.txt
```

this command works for directories as well.

## chgrp
we must be the owner of the file or directory to perform this operation.

```bash
chgrp new_groupname file.txt
```
