# Day 10 – File Permissions & File Operations Challenge

## Challenge Tasks

### Task 1: Create Files (10 minutes)

1. Create empty file `devops.txt` using `touch` => touch devops.txt

2. Create `notes.txt` with some content using `cat` or `echo` 
=> echo "Hello Doston!!" > notes.txt

3. Create `script.sh` using `vim` with content: `echo "Hello DevOps"`
=> vim script.sh
echo "Hello Devops"
chmod +x script.sh
cat script.sh

-----------------------------------------------------------------------------
### Task 2: Read Files (10 minutes)

1. Read `notes.txt` using `cat`
=> cat notes.txt

2. View `script.sh` in vim read-only mode
=> chmod 400 script.sh
vim script.sh

3. Display first 5 lines of `/etc/passwd` using `head`
=> cat /etc/passwd | head -5

4. Display last 5 lines of `/etc/passwd` using `tail`
=> cat /etc/passwd | tail -5

--------------------------------------------------------------------------------------

### Task 3: Understand Permissions (10 minutes)

Format: `rwxrwxrwx` (owner-group-others)
- `r` = read (4), `w` = write (2), `x` = execute (1)

Check your files: `ls -l devops.txt notes.txt script.sh`
=> ls -l

Answer: What are current permissions? Who can read/write/execute?
=> ls -l
total 12
-rw-rw-r-- 1 ubuntu ubuntu 2346 Feb  5 23:36 README.md
-rw-rw-r-- 1 ubuntu ubuntu    0 Mar  1 10:35 devops.txt
### owner = read-write
### group = read-write
### others = only read permissons

-rw-rw-r-- 1 ubuntu ubuntu   13 Mar  1 10:37 notes.txt
### owner = read-write
### group = read-write
### others = only read permissons

-r-------- 1 ubuntu ubuntu   32 Mar  1 10:42 script.sh
### owner = read only permissons
### group = neither read nor write & execute
### others = neither read nor write & execute

------------------------------------------------------------------------------------------------------

### Task 4: Modify Permissions (20 minutes)

1. Make `script.sh` executable → run it with `./script.sh`
=> chmod 500 script.sh

2. Set `devops.txt` to read-only (remove write for all)
=> chmod 400 devops.txt

3. Set `notes.txt` to `640` (owner: rw, group: r, others: none)
=> chmod 640 notes.txt

4. Create directory `project/` with permissions `755`
=> mkdir -m 755 project/


**Verify:** `ls -l` after each change
ls -l
total 16
-rw-rw-r-- 1 ubuntu ubuntu 2346 Feb  5 23:36 README.md
-r-------- 1 ubuntu ubuntu    0 Mar  1 10:35 devops.txt
-rw-r----- 1 ubuntu ubuntu   13 Mar  1 10:37 notes.txt
drwxr-xr-x 2 ubuntu ubuntu 4096 Mar  1 11:08 project
-r-x------ 1 ubuntu ubuntu   32 Mar  1 10:42 script.sh

-------------------------------------------------------------------------------------------------------------

### Task 5: Test Permissions (10 minutes)

1. Try writing to a read-only file - what happens?
E45: 'readonly' option is set (add ! to override) 

2. Try executing a file without execute permission
=> ./devops.txt

3. Document the error messages
-bash: ./devops.txt: Permission denied