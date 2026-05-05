# 👥 User & Group Management in Linux (Advanced Study Guide)

---

# 📌 1. Core Concepts (Quick Recap)

* Linux is **multi-user + multi-tasking**
* Each user has:

  * **UID (User ID)**
  * **GID (Group ID)**
* Permissions are controlled via:

  * **User (Owner)**
  * **Group**
  * **Others**

---

# 👤 2. Types of Users (With Commands)

### 🔴 Root User

```bash
id
whoami
```

👉 Check UID:

```bash
id root
```

---

### ⚙️ System Users (Service Accounts)

```bash
cat /etc/passwd | grep -E 'nologin|false'
```

---

### 👨‍💻 Regular Users

```bash
id username
```

---

# ➕ 3. Advanced User Management Commands

## 🔹 Create Users (More Options)

```bash
sudo useradd -m -c "DevOps User" -s /bin/bash username
```

👉 With specific UID:

```bash
sudo useradd -u 1050 username
```

👉 With expiry date:

```bash
sudo useradd -e 2026-12-31 username
```

👉 With no login shell:

```bash
sudo useradd -s /sbin/nologin username
```

---

## 🔹 Password & Account Management

```bash
sudo passwd username
```

👉 Lock user:

```bash
sudo passwd -l username
```

👉 Unlock user:

```bash
sudo passwd -u username
```

👉 Force password change:

```bash
sudo passwd -e username
```

---

## 🔹 Account Expiry & Aging

```bash
sudo chage -l username
```

👉 Set expiry:

```bash
sudo chage -E 2026-12-31 username
```

👉 Password expiry:

```bash
sudo chage -M 90 username
sudo chage -m 7 username
sudo chage -W 7 username
```

---

## 🔹 Modify User (Advanced)

```bash
sudo usermod -aG devops username
sudo usermod -L username     # Lock account
sudo usermod -U username     # Unlock account
```

👉 Change shell:

```bash
sudo usermod -s /bin/zsh username
```

👉 Change UID:

```bash
sudo usermod -u 2000 username
```

---

## 🔹 Delete User (Safe Cleanup)

```bash
sudo userdel -r username
```

👉 Force delete:

```bash
sudo userdel -f username
```

---

# 👥 4. Advanced Group Management

## 🔹 Create Group with GID

```bash
sudo groupadd -g 2001 devops
```

---

## 🔹 Modify Group

```bash
sudo groupmod -n newgroup oldgroup
```

---

## 🔹 Manage Group Membership

```bash
sudo gpasswd -a username groupname   # Add user
sudo gpasswd -d username groupname   # Remove user
```

---

## 🔹 Set Group Password (rare use)

```bash
sudo gpasswd groupname
```

---

## 🔹 Change Primary Group

```bash
sudo usermod -g groupname username
```

---

# 📂 5. Important Files (Deep Understanding)

## 📄 /etc/passwd

```bash
cat /etc/passwd
```

👉 Fields:

```
username:x:UID:GID:comment:home:shell
```

---

## 🔐 /etc/shadow

```bash
sudo cat /etc/shadow
```

👉 Fields:

```
username:encrypted_password:last_change:min:max:warn:inactive:expire
```

---

## 👥 /etc/group

```bash
cat /etc/group
```

---

## 🆕 /etc/login.defs (Important)

```bash
cat /etc/login.defs
```

👉 Controls:

* Default UID range
* Password aging
* Home directory settings

---

# 🔄 6. Switching Users & Privileges (Advanced)

## 🔹 Switch User

```bash
su username
su - username
```

👉 Difference:

* `su` → keeps environment
* `su -` → loads full environment

---

## 🔹 Sudo Commands

```bash
sudo -i        # Login shell as root
sudo -l        # Check permissions
```

---

## 🔹 Add User to Sudoers

```bash
sudo usermod -aG sudo username   # Ubuntu
sudo usermod -aG wheel username  # RHEL
```

---

## 🔹 Edit Sudo File

```bash
sudo visudo
```

👉 Example:

```
username ALL=(ALL) NOPASSWD:ALL
```

---

# 📊 7. User Monitoring & Audit Commands

```bash
who
w
last
lastlog
```

👉 Active sessions:

```bash
who -a
```

---

👉 Logged-in users:

```bash
users
```

---

👉 Check login history:

```bash
last -n 10
```

---

# 🔐 8. Permissions & Ownership (Must Know)

## 🔹 Change Ownership

```bash
sudo chown user:group file.txt
```

---

## 🔹 Change Permissions

```bash
chmod 755 file.sh
chmod u+x file.sh
```

---

## 🔹 Recursive Changes

```bash
sudo chown -R user:group folder/
chmod -R 755 folder/
```

---

# ⚙️ 9. Default File Permissions (umask)

```bash
umask
```

👉 Set:

```bash
umask 022
```

👉 Meaning:

* Default file permissions = 666 - umask
* Directory permissions = 777 - umask

---

# 🔧 10. Bulk User Management (Important for DevOps)

## 🔹 Create Multiple Users

```bash
for user in dev1 dev2 dev3
do
  sudo useradd -m $user
  echo "$user:Password123" | sudo chpasswd
done
```

---

## 🔹 Delete Multiple Users

```bash
for user in dev1 dev2 dev3
do
  sudo userdel -r $user
done
```

---

# 🛡️ 11. Security & Best Practices (VERY IMPORTANT)

### 🔐 Access Control

* Never enable root SSH login ❌
* Use `sudo` instead of root login ✅
* Implement **least privilege**

---

### 👥 User Policies

* Disable unused accounts:

```bash
sudo usermod -L username
```

* Set expiry for temporary users

---

### 🔑 Password Policies

* Use strong password rules
* Enable password aging
* Use `/etc/login.defs`

---

### 📊 Monitoring

* Track login attempts:

```bash
last
```

* Check failed logins:

```bash
sudo cat /var/log/auth.log | grep "Failed"
```

---

### 🔒 File Security

* Restrict sensitive files:

```bash
chmod 600 /etc/shadow
```

---

# ⚡ 12. Quick Command Cheat Sheet

| Action            | Command                      |
| ----------------- | ---------------------------- |
| Create user       | `useradd -m username`        |
| Set password      | `passwd username`            |
| Lock user         | `passwd -l username`         |
| Delete user       | `userdel -r username`        |
| Create group      | `groupadd groupname`         |
| Add user to group | `usermod -aG group username` |
| Switch user       | `su - username`              |
| Run as root       | `sudo command`               |
| Check users       | `who`, `w`                   |
| Login history     | `last`                       |

---

# 🎯 Final Important Points (Interview Focus)

* UID 0 = root 🔴
* `/etc/passwd` → user info
* `/etc/shadow` → encrypted passwords
* `/etc/group` → group mapping
* `sudo` is safer than `su`
* `umask` controls default permissions
* `chage` manages password expiry
* `visudo` prevents syntax errors
* Always use **groups for scalable access control**
* Follow **Principle of Least Privilege (PoLP)**

---
