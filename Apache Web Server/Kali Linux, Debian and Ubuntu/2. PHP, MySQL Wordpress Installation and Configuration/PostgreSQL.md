
# ✅ Install and Configure PostgreSQL with LDAP (Debian/Kali/Ubuntu)

### 🔧 Step 1: Install PostgreSQL

```bash
apt update
apt install postgresql postgresql-contrib
```

### 🔍 Step 2: Check PostgreSQL Service

```bash
systemctl status postgresql
```

### 🛠️ Step 3: Configure LDAP Authentication

Edit `pg_hba.conf`:

```bash
vim /etc/postgresql/*/main/pg_hba.conf
```

Add this line at the bottom:

```
host    all             all             0.0.0.0/0               ldap ldapserver=192.168.169.130 ldapbasedn="ou=People,dc=server,dc=local"
```

### 🛠️ Step 4: Allow External Connections

Edit `postgresql.conf`:

```bash
vim /etc/postgresql/*/main/postgresql.conf
```

Uncomment and set:

```
listen_addresses = '*'
```

### 🔄 Step 5: Restart and Verify Service

```bash
systemctl restart postgresql
systemctl status postgresql
```

### 🔗 Step 6: Connect to PostgreSQL via LDAP

```bash
psql -h 192.168.169.130 -U nikhil -d postgres
```

```
su - postgres
psql
```
```
show result
postgres=#
```
```
Creating nikhil Role in PostgreSQL
```
```
CREATE ROLE nikhil WITH SUPERUSER LOGIN PASSWORD 'Patidar@123';
```
---

### 📚 PostgreSQL Commands Cheat Sheet

#### 💾 Database Management

| Command                 | काम (Description)             |
| ----------------------- | ----------------------------- |
| `\l`                    | सभी databases की सूची दिखाएं  |
| `\c dbname`             | किसी database से connect करें |
| `CREATE DATABASE mydb;` | नया database बनाएं            |
| `DROP DATABASE mydb;`   | database को delete करें       |

#### 👤 User Management

| Command                                   | काम (Description)              |
| ----------------------------------------- | ------------------------------ |
| `\du`                                     | सभी users/roles की सूची दिखाएं |
| `CREATE USER user1 WITH PASSWORD 'pass';` | नया user बनाएं                 |
| `DROP USER user1;`                        | user को delete करें            |
| `ALTER USER user1 WITH SUPERUSER;`        | किसी user को superuser बनाएं   |

#### 📋 Table Management

| Command                                                     | काम (Description)         |
| ----------------------------------------------------------- | ------------------------- |
| `\dt`                                                       | सभी tables की सूची दिखाएं |
| `CREATE TABLE students (id SERIAL PRIMARY KEY, name TEXT);` | नई table बनाएं            |
| `DROP TABLE students;`                                      | table को delete करें      |
| `\d students`                                               | table की structure दिखाएं |

---
