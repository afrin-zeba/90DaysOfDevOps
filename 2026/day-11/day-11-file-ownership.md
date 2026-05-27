## Challenge Tasks

### Task 1: Understanding Ownership (10 minutes)

1. Run `ls -l` in your home directory
ubuntu@ip-172-31-46-159:/home$ ls -ltrh
total 24K
drwxr-x--- 5 ubuntu    ubuntu    4.0K May 27 07:34 ubuntu
drwxr-x--- 2 tokyo     tokyo     4.0K May 27 12:37 tokyo
drwxr-x--- 2 berlin    berlin    4.0K May 27 12:37 berlin
drwxr-x--- 2 professor professor 4.0K May 27 12:37 professor

2. Identify the **owner** and **group** columns
3. Check who owns your files

**Format:** `-rw-r--r-- 1 owner group size date filename`

Document: What's the difference between owner and group? --> owner is usually the creator/owner of the file (user) whereas group can contain multiple users (team)

---

### Task 2: Basic chown Operations (20 minutes)

1. Create file `devops-file.txt`
touch devops-file.txt
2. Check current owner: `ls -l devops-file.txt`
-rw-r--r-- 1 root      root         0 May 27 15:34 devops-file.txt
3. Change owner to `tokyo` (create user if needed)
chown tokyo: devops-file.txt
4. Change owner to `berlin`
chown berlin: devops-file.txt
5. Verify the changes
-rw-r--r-- 1 berlin    root         0 May 27 15:34 devops-file.txt

**Try:**
```bash
sudo chown tokyo devops-file.txt
```

---

### Task 3: Basic chgrp Operations (15 minutes)

1. Create file `team-notes.txt`
touch team-notes.txt
2. Check current group: `ls -l team-notes.txt`
-rw-r--r-- 1 root  root   0 May 27 15:36 team-notes.txt
3. Create group: `sudo groupadd heist-team`
sudo groupadd heist-team 
4. Change file group to `heist-team`
chown :heist-team team-notes.txt
5. Verify the change
-rw-r--r-- 1 root      heist-team    0 May 27 15:36 team-notes.txt
---

### Task 4: Combined Owner & Group Change (15 minutes)

Using `chown` you can change both owner and group together:

1. Create file `project-config.yaml`
touch project-config.yaml
2. Change owner to `professor` AND group to `heist-team` (one command)
chown professor:heist-team project-config.yaml
3. Create directory `app-logs/`
mkdir app-logs
4. Change its owner to `berlin` and group to `heist-team`
chown berlin:heist-team app-logs/

**Syntax:** `sudo chown owner:group filename`

---

### Task 5: Recursive Ownership (20 minutes)

1. Create directory structure:
   ```
   mkdir -p heist-project/vault
   mkdir -p heist-project/plans
   touch heist-project/vault/gold.txt
   touch heist-project/plans/strategy.conf
   ```

2. Create group `planners`: `sudo groupadd planners`

3. Change ownership of entire `heist-project/` directory:
   - Owner: `professor`
   - Group: `planners`
   - Use recursive flag (`-R`)
chown -R professor:planners heist-project/
4. Verify all files and subdirectories changed: `ls -lR heist-project/`

---

### Task 6: Practice Challenge (20 minutes)

1. Create users: `tokyo`, `berlin`, `nairobi` (if not already created)
sudo -m useradd tokyo 
sudo -m useradd berlin 
sudo -m useradd nairobi 
2. Create groups: `vault-team`, `tech-team`
sudo groupadd vault-team
sudo groupadd tech-team
3. Create directory: `bank-heist/`
mkdir bank-heist/
4. Create 3 files inside:
   ```
   touch bank-heist/access-codes.txt
   touch bank-heist/blueprints.pdf
   touch bank-heist/escape-plan.txt
   ```

5. Set different ownership:
   - `access-codes.txt` → owner: `tokyo`, group: `vault-team`
   chown tokyo:vault-team access-codes.txt
   - `blueprints.pdf` → owner: `berlin`, group: `tech-team`
   chown berlin:tech-team blueprints.pdf
   - `escape-plan.txt` → owner: `nairobi`, group: `vault-team`
   chown nairobi:vault-team escape-plan.txt
**Verify:** `ls -l bank-heist/`
-rw-r--r-- 1 tokyo   vault-team 0 May 27 15:42 access-codes.txt
-rw-r--r-- 1 berlin  tech-team  0 May 27 15:42 blueprints.pdf
-rw-r--r-- 1 nairobi vault-team 0 May 27 15:42 escape-plan.txt
---

## Key Commands Reference

```bash
# View ownership
ls -l filename

# Change owner only
sudo chown newowner filename

# Change group only
sudo chgrp newgroup filename

# Change both owner and group
sudo chown owner:group filename

# Recursive change (directories)
sudo chown -R owner:group directory/

# Change only group with chown
sudo chown :groupname filename
```

---

## Hints

- Most `chown`/`chgrp` operations need `sudo`
- Use `-R` flag for recursive directory changes
- Always verify with `ls -l` after changes
- User must exist before using in `chown`
- Group must exist before using in `chgrp`/`chown`

---





