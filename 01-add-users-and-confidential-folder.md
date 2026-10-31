# Ticket 01: Add Users and Create a Confidential Folder

## Ticket

![Ticket](images/01-ticket.png)

> Hello Admin, please add two users to the Ubuntu Linux Host, Bertram and Erlich (you can leave all additional details such as last name, room number and phone numbers blank). Please create a password for each of them and expire their password so they need to create one on their first login. Also please create a new folder in the root directory called "Confidential". Make root the owner of the folder with full permissions, however, allow Bertram and Erlich full permissions to the folder. Ensure that everyone else on the computer has no permissions so they can't access the folder. Thanks!

**Tasks**

1. Add users Bertram and Erlich (leave extra details blank)
2. Set a password for each and expire it so they must change it at first login
3. Create `/Confidential`
4. Root owns it with full permissions, Bertram and Erlich get full permissions, everyone else gets none

## Solution

### 1. Add the users

```bash
sudo adduser Bertram
sudo adduser Erlich
```

I set a temporary password for each and pressed Enter through the Full Name, Room Number, and phone prompts to leave them blank, then confirmed with `y`.

![adduser Bertrom](images/01-adduser-1.png)
![adduser Erlich](images/01-adduser-2.png)

Both accounts show up on the Ubuntu login screen:

![Login screen](images/01-login-screen.png)

### 2. Expire the passwords

```bash
sudo passwd --expire Bertram
sudo passwd --expire Erlich
```

This forces each user to choose a new password the first time they log in.

![Expire passwords](images/01-expire-passwords.png)

Optional check:

```bash
sudo chage -l Bertram
```

"Last password change" shows `password must be changed`.

### 3. Create the Confidential folder in /

```bash
cd /
sudo mkdir Confidential
ls
```

Created with `sudo`, so root owns it by default.

![mkdir Confidential](images/01-mkdir-confidential.png)

### 4. Give Bertram and Erlich access, lock out everyone else

Users get access through a shared group, so the folder is owned by `root`, group-owned by `Employees`, and closed to everyone else:

```bash
sudo groupadd Employees
sudo usermod -aG Employees Bertram
sudo usermod -aG Employees Erlich
sudo chown root:Employees /Confidential
sudo chmod 770 /Confidential
```

- Owner (root): read, write, execute
- Group (Employees: Bertram and Erlich): read, write, execute
- Others: no access

### 5. Verify

```bash
ls -ld /Confidential
```

Result: `drwxrwx--- 2 root Employees 4096 Oct 31 05:44 /Confidential`

![Verify permissions](images/01-verify-permissions.png)

## Mistakes and lessons

- **Typos in usernames.** The ticket asks for **Bertram**, but I created the account as **Bertrom**. The first `passwd --expire Bertom` also failed with "user does not exist" (visible in the screenshot above). Always re-read the ticket for exact spellings, and check with `getent passwd <name>` before moving on. A misspelled account can be renamed with `sudo usermod -l Bertram Bertrom`.
- **Typo in the path.** `ls -ld /Cofidential` returned "No such file or directory". Linux paths are case-sensitive and exact. Tab completion avoids this.
- **Leftover home directories.** `adduser` warned that `/home/<name>` already existed from an earlier deleted account. Delete accounts with `sudo deluser --remove-home <name>`, and remove orphaned folders with `sudo rm -rf /home/<name>`.

## Commands used

```bash
ls -ld /path            # owner, group, permissions of a directory
getent group <group>    # does a group exist, who is in it
getent passwd <user>    # does a user exist
id <user>               # a user's groups
```
