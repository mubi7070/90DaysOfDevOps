# Week 2: Linux System Administration & Automation

Welcome to **Week 2** of the **90 Days of DevOps - 2025 Edition**! This week, we dive into **Linux system administration and automation**, covering essential topics such as **user management, file permissions, log analysis, process control, volume mounts, and shell scripting**.

---

## 🚀 Project: DevOps Linux Server Monitoring & Automation
Imagine you're managing a **Linux-based production server** and need to ensure that **users, logs, and processes** are well-managed. You will perform real-world tasks such as **log analysis, volume management, and automation** to enhance your DevOps skills.

---

## 📌 Tasks

### **1️⃣ User & Group Management**
- Learn about Linux **users, groups, and permissions** (`/etc/passwd`, `/etc/group`).
- **Task with answer scripts:**  
  - Create a user `devops_user` and add them to a group `devops_team`.

        sudo useradd -m devops_user -s /bin/bash
        sudo groupadd devops_team
        sudo usermod -aG devops_team devops_user
        cat /etc/passwd

  - Set a password and grant **sudo** access.

        sudo passwd devops_user
        sudo usermod -aG sudo devops_user
        cat /etc/passwd | grep "devops_user"
    
  - Restrict SSH login for certain users in `/etc/ssh/sshd_config`.

        sudo vim /etc/ssh/sshd_config

Add these variables in Host*:

        AllowUsers user1 user2 user3
    
Command:

        cat sshd_config | grep -i "AllowUsers"
        sudo systemctl restart ssh
    
---

### **2️⃣ File & Directory Permissions**
- **Task:**  
  - Create `/devops_workspace` and a file `project_notes.txt`.

        mkdir devops_workspace
        touch project_notes.txt
        ls
              
  - Set permissions:
  - **Owner can edit**, **group can read**, **others have no access**.
   
        sudo chown ubuntu:ubuntu devops_workspace/
        chmod 640 project_notes.txt

  - Use `ls -l` to verify permissions.

    Result:

        ubuntu@ip-172-31-0-130:~/devops_workspace$ ls -l
        -rw-r----- 1 ubuntu ubuntu 0 Feb 15 16:15 project_notes.txt

---

### **3️⃣ Log File Analysis with AWK, Grep & Sed**
Logs are crucial in DevOps! You’ll analyze logs using the **Linux_2k.log** file from **LogHub** ([GitHub Repo](https://github.com/logpai/loghub/blob/master/Linux/Linux_2k.log)).

- **Task:**  
  - **Download the log file** from the repository.
 
        wget https://github.com/logpai/loghub/blob/master/Linux/Linux_2k.log
    
  - **Extract insights using commands:**
    - Use `grep` to find all occurrences of the word **authentication failure**.
      
          cat Linux_2k.log | grep -i "error"
        
    - Use `awk` to extract **timestamps and log levels**.
                   
          awk '/authentication failure/ {print $3,$5}' Linux_2k.log
      
    - Use `sed` to replace all IP addresses with **[REDACTED]** for security.

      
          awk '/authentication failure/ {print $3,$5,$6,$7,$13}' Linux_2k.log > Output.txt
          sed -E 's/([0-9]{1,3}\.){3}[0-9]{1,3}/[REDACTED]/g' Output.txt
          
      
  - **Bonus:** Find the most frequent log entry using `awk` or `sort | uniq -c | sort -nr | head -10`.

          awk '/authentication failure/ {print $6,$7,$13}' Linux_2k.log | uniq -c | sort -nr | head -n 20
---

### **4️⃣ Volume Management & Disk Usage**
- **Task:**  
  - Create a directory `/mnt/devops_data`.
  - Mount a new volume (or loop device for local practice).
  - Verify using `df -h` and `mount | grep devops_data`.

---

### **5️⃣ Process Management & Monitoring**
- **Task:**  
  - Start a background process (`ping google.com > ping_test.log &`).
  - Use `ps`, `top`, and `htop` to monitor it.
  - Kill the process and verify it's gone.

---

### **6️⃣ Automate Backups with Shell Scripting**
- **Task:**  
  - Write a shell script to back up `/devops_workspace` as `backup_$(date +%F).tar.gz`.
  - Save it in `/backups` and schedule it using `cron`.
  - Make the script display a success message in **green text** using `echo -e`.

---

## 🎯 Bonus Tasks (Optional 🚀)
1. Find the **top 5 most common log messages** in `Linux_2k.log` using `awk` and `sort`.
2. Use `find` to list **all files modified in the last 7 days**.
3. Write a script that extracts and displays only **ERROR and WARNING logs** from `Linux_2k.log`.

---

## 📢 How to Submit
- **Write a LinkedIn post** summarizing your Week 2 experience.
- Include screenshots or logs of your tasks.
- **Use hashtags**: `#90DaysOfDevOps` `#LinuxAdmin` `#DevOps`
- Share any blog posts, GitHub repos, or articles you create.

---

## 📚 Resources to Get Started
- [Linux In One Shot](https://youtu.be/e01GGTKmtpc?si=FSVNFRwdNC0NZeba)
- [Linux_2k.log (LogHub)](https://github.com/logpai/loghub/blob/master/Linux/Linux_2k.log)

---

## 📝 Example Submission Post
```markdown
Week 2 of #90DaysOfDevOps2025 done! 🏆

✅ Managed users & SSH access  
✅ Set up permissions & volumes  
✅ Analyzed logs using AWK & grep  
✅ Automated backups with a shell script  

Check out my blog here: [Your Blog/GitHub Link]  

#Linux #SysAdmin #DevOps
```

---

Happy learning, and see you in **Week 3**! 🚀

