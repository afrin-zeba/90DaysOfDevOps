## Challenge Tasks

### Task 1: Create Users (20 minutes)

Create three users with home directories and passwords:
- `tokyo`
- `berlin`
- `professor`

**Verify:** Check `/etc/passwd` and `/home/` directory

sudo useradd -m tokyo
sudo passwd tokyo

sudo adduser berlin
sudo adduser professor

## add user vs useradd 
sudo useradd -m tokyo

does:

create user tokyo
create home directory (-m)

But it does not prompt for a password automatically. You must set the password separately: sudo passwd tokyo Then you'll be prompted to enter a password.

sudo adduser tokyo

This:

creates the user
creates home directory
asks for password immediately
asks optional details like full name
---

### Task 2: Create Groups (10 minutes)

Create two groups:
- `developers`
- `admins`

**Verify:** Check `/etc/group`

sudo groupadd developers
sudo groupadd admins

ubuntu@ip-172-31-46-159:~$ cat /etc/group | grep developers
developers:x:1008:
ubuntu@ip-172-31-46-159:~$ cat /etc/group | grep admins
admins:x:1009:
---

### Task 3: Assign to Groups (15 minutes)

Assign users:
- `tokyo` → `developers`
- `berlin` → `developers` + `admins` (both groups)
- `professor` → `admins`

**Verify:** Use appropriate command to check group membership

sudo gpasswd -a tokyo developers
sudo gpasswd -a berlin developers
sudo gpasswd -a berlin admins
sudo gpasswd -a professor admins

ubuntu@ip-172-31-46-159:~$ cat /etc/group | grep developers
developers:x:1008:tokyo,berlin
ubuntu@ip-172-31-46-159:~$ cat /etc/group | grep admins
admins:x:1009:berlin,professor
---

### Task 4: Shared Directory (20 minutes) 

## syntax for changing owner and group 
chown owner:group file
only owner --> chown owner: file
only group --> chown :group file

1. Create directory: `/opt/dev-project`
mkdir /opt/dev-project
2. Set group owner to `developers`
sudo chown :developers dev-project
3. Set permissions to `775` (rwxrwxr-x)
chmod 775 dev-project 
4. Test by creating files as `tokyo` and `berlin`
su tokyo 
touch tokyo.txt

**Verify:** Check permissions and test file creation

---

### Task 5: Team Workspace (20 minutes)

1. Create user `nairobi` with home directory
sudo useradd -m nairobi 
2. Create group `project-team`
sudo groupadd project-team
3. Add `nairobi` and `tokyo` to `project-team`
sudo gpasswd -a nairobi project-team
sudo gpasswd -a nairobi project-team
4. Create `/opt/team-workspace` directory
mkdir /opt/team-workspace
5. Set group to `project-team`, permissions to `775`
chown :project-team team-workspace 
chmod 775 team-workspace
6. Test by creating file as `nairobi`
su nairobi
cd team-workspace
touch nairobi.txt

---

## Hints

**Stuck? Try these commands:**
- User: `useradd`, `passwd`, `usermod`
- Group: `groupadd`, `groups`
- Permissions: `chgrp`, `chmod`
- Test: `sudo -u username command`

**Tip:** Use `-m` flag with useradd for home directory, `-aG` for adding to groups


