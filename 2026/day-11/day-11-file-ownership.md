# Day 11 – File Ownership Challenge (chown & chgrp)

## Challenge Tasks

### Task 1: Understanding Ownership (10 minutes)

1. Run `ls -l` in your home directory
=> ls -l
total 8
drwxrwxr-x 9 ubuntu ubuntu 4096 Feb  5 23:36 90DaysOfDevOps
-rw-rw-r-- 1 ubuntu ubuntu  414 Feb  5 22:37 nginx_logs.txt


2. Identify the **owner** and **group** columns
## owner = ubuntu
## group = ubuntu

3. Check who owns your files

**Format:** `-rw-r--r-- 1 owner group size date filename`

drwxrwxr-x 9 ubuntu ubuntu 4096 Feb  5 23:36 90DaysOfDevOps
-rw-rw-r-- 1 ubuntu ubuntu  414 Feb  5 22:37 nginx_logs.txt

Document: What's the difference between owner and group?

# owner = the user that creates a file or folder becomed owner by default. it have special control on that file/folder.
To change the owner of a file => chown username filename

# Group = group is a collection of multiple user. A file should have at least one assigned group and member of that group gets all the write of that group.
To Change the group of a file => chgrp groupname filename

To change All by one => chown user:group filename

---------------------------------------------------------------------------------------

### Task 2: Basic chown Operations (20 minutes)

1. Create file `devops-file.txt`
=> touch devops-file.txt

2. Check current owner: `ls -l devops-file.txt`

3. Change owner to `tokyo` (create user if needed)
=> sudo useradd tokyo
sudo chown tokyo devops-file.txt

ls -l
total 8
-rw-rw-r-- 1 ubuntu ubuntu 4448 Feb  5 23:36 README.md
-rw-rw-r-- 1 tokyo  ubuntu    0 Mar  1 13:46 devops-file.txt

4. Change owner to `berlin`
=> sudo chown berlin devops-file.txt

5. Verify the changes
ls -l
total 8
-rw-rw-r-- 1 ubuntu ubuntu 4448 Feb  5 23:36 README.md
-rw-rw-r-- 1 berlin ubuntu    0 Mar  1 13:46 devops-file.txt

**Try:**
```bash
sudo chown tokyo devops-file.txt
```

---------------------------------------------------------------------------------------

### Task 3: Basic chgrp Operations (15 minutes)

1. Create file `team-notes.txt`
touch team-notes.txt

2. Check current group: `ls -l team-notes.txt`
ls -l
total 8
-rw-rw-r-- 1 ubuntu ubuntu 4448 Feb  5 23:36 README.md
-rw-rw-r-- 1 berlin ubuntu    0 Mar  1 13:46 devops-file.txt
-rw-rw-r-- 1 ubuntu ubuntu    0 Mar  1 14:23 team-notes.txt

3. Create group: `sudo groupadd heist-team`

4. Change file group to `heist-team`
ls -l
total 8
-rw-rw-r-- 1 ubuntu ubuntu     4448 Feb  5 23:36 README.md
-rw-rw-r-- 1 berlin ubuntu        0 Mar  1 13:46 devops-file.txt
-rw-rw-r-- 1 ubuntu heist-team    0 Mar  1 14:23 team-notes.txt

5. Verify the change

---------------------------------------------------------------------------------------

### Task 4: Combined Owner & Group Change (15 minutes)

Using `chown` you can change both owner and group together:

1. Create file `project-config.yaml`
touch project-config.yaml

2. Change owner to `professor` AND group to `heist-team` (one command)
sudo chown professor:heist-team project-config.yaml

3. Create directory `app-logs/`
=> mkdir app-logs

4. Change its owner to `berlin` and group to `heist-team`
sudo chown berlin:heist-team app-logs

**Syntax:** `sudo chown owner:group filename`

---------------------------------------------------------------------------------------

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

4. Verify all files and subdirectories changed: `ls -lR heist-project/`

---------------------------------------------------------------------------------------

### Task 6: Practice Challenge (20 minutes)

1. Create users: `tokyo`, `berlin`, `nairobi` (if not already created)
2. Create groups: `vault-team`, `tech-team`
sudo groupadd vault-team
sudo groupadd tech-team

3. Create directory: `bank-heist/`
mkdir bank-heist

4. Create 3 files inside:
   ```
   touch bank-heist/access-codes.txt
   touch bank-heist/blueprints.pdf
   touch bank-heist/escape-plan.txt
   ```

ls -l
total 0
-rw-rw-r-- 1 ubuntu ubuntu 0 Mar  1 14:57 access-codes.txt
-rw-rw-r-- 1 ubuntu ubuntu 0 Mar  1 14:57 blueprints.pdf
-rw-rw-r-- 1 ubuntu ubuntu 0 Mar  1 14:57 escape-plan.txt



5. Set different ownership:
   - `access-codes.txt` → owner: `tokyo`, group: `vault-team`
   - `blueprints.pdf` → owner: `berlin`, group: `tech-team`
   - `escape-plan.txt` → owner: `nairobi`, group: `vault-team`

sudo chown tokyo:vault-team access-codes.txt
sudo chown berlin:tech-team blueprints.pdf
sudo chown nairobi:vault-team escape-plan.txt

**Verify:** `ls -l bank-heist/`

ls -l
total 0
-rw-rw-r-- 1 tokyo   vault-team 0 Mar  1 14:57 access-codes.txt
-rw-rw-r-- 1 berlin  tech-team  0 Mar  1 14:57 blueprints.pdf
-rw-rw-r-- 1 nairobi vault-team 0 Mar  1 14:57 escape-plan.txt
