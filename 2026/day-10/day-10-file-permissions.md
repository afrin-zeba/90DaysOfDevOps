### Task 1: Create Files (10 minutes)

1. Create empty file `devops.txt` using `touch` --> touch devops.txt
2. Create `notes.txt` with some content using `cat` or `echo` --> 
vi notes.txt 
echo "this is notes.txt" >> notes.txt | cat notes.txt 
3. Create `script.sh` using `vim` with content: `echo "Hello DevOps"` --> 
vim script.sh 
#!/bin/bash 
echo "Hello DevOps"
**Verify:** `ls -l` to see permissions

---

### Task 2: Read Files (10 minutes)

1. Read `notes.txt` using `cat`
2. View `script.sh` in vim read-only mode
3. Display first 5 lines of `/etc/passwd` using `head`
4. Display last 5 lines of `/etc/passwd` using `tail`

cat notes.txt
vim script.sh
cat /etc/passwd | head -n 5 
cat /etc/passwd | tail -n 5 
---

### Task 3: Understand Permissions (10 minutes)

Format: `rwxrwxrwx` (owner-group-others)
- `r` = read (4), `w` = write (2), `x` = execute (1)

Check your files: `ls -l devops.txt notes.txt script.sh`

Answer: What are current permissions? Who can read/write/execute?

-rw-rw-r-- 1 ubuntu ubuntu    0 May 27 06:30 devops.txt 
-rw-rw-r-- 1 ubuntu ubuntu   18 May 27 06:38 notes.txt
-rw-rw-r-- 1 ubuntu ubuntu    0 May 27 06:38 script.sh
user and groups have read + write permissions , other users have only read permissions 
---

### Task 4: Modify Permissions (20 minutes)

1. Make `script.sh` executable → run it with `./script.sh`
2. Set `devops.txt` to read-only (remove write for all)
3. Set `notes.txt` to `640` (owner: rw, group: r, others: none)
4. Create directory `project/` with permissions `755`

**Verify:** `ls -l` after each change
chmod 764 script.sh 
./script.sh

chmod -wx devops.txt 
chmod +r devops.txt

chmod 640 notes.txt

mkdir project 
chmod 755 project/
---

### Task 5: Test Permissions (10 minutes)

1. Try writing to a read-only file - what happens? --> Warning : changing a read only file 
2. Try executing a file without execute permission --> -bash: ./read_only.txt: Permission denied
3. Document the error messages

