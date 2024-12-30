
### Table of contents
1. [User management](#user-management)
2. [Group management](#group-management)
3. [User info](#user-information)
4. [Root user](#root-user)
5. [Navigation](#navigation)


Most commands operate like this: ```command -options arguments```

## User management

### 1. Create a new user

```bash
sudo adduser username
```

### 2. Set password for the new user / Change user's password

```bash
sudo passwd username
```

### 3. Add user to a group

```bash
sudo usermod -aG groupname username
```

### 4. Delete a user

```bash
sudo deluser username
```

### 5. Delete a user along with their home directory

```bash
sudo deluser --remove-home username
```

### 7. Lock a user account

```bash
sudo passwd -l username
```

### 8. Unlock a user account

```bash
sudo passwd -u username
```

## Group management

### 1. Create a new group

```bash
sudo addgroup groupname
```

### 2. Delete a group

```bash
sudo delgroup groupname
```

### 3. Add a user to a group

```bash
sudo usermod -aG groupname username
```

### 4. Remove a user from a group

```bash
sudo deluser username groupname
```

### 5. List all groups

```bash
cat /etc/group
```

### 6. List all groups a user belongs to

```bash
groups username
```

## User information

### 1. Display user information

```bash
id username
```

### 2. Display user's home directory

```bash
echo ~username
```

## Root user

### 1. Switch to root user

```bash
sudo su
```


**Example:**
You enter the container as root user.
To experiment with locking and unlocking user accounts, you need to operate from non-root users.
Update root password to something you know, then create a bunch of new user and set a password for the new user.

```bash
passwd root

adduser user1 # addition flow for user1

adduser user2 # addition flow for user2

su user1

# lock / unlock other user accounts
...

```

## Navigation
Common commands for navigation: ```pwd, cd, ls, less, file```

**pwd** -- shows the name of the working directory

**cd**
```bash
cd /usr/bin # common navigation
cd ..       # wd to parent dir with a relative pathname
cd ./bin    # wd to child dir with a relative pathname
cd -        # changes the working directory to the previous one
cd          # followed by nothing - sends to home dir
cd ~user_name # wd to home dir of the specified user
```

**ls** -- lists files and directories

```bash
ls -a  # also lists hidden files
ls -l  # lists in a long format
ls -la # combination of above features
ls <dir1> <dir2> # lists files from both dirs
```

**less** -- view contents of text files. When in view mode, press **q** to quit

**file** -- determine file's content

