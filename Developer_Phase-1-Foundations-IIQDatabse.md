# 🗄️ SailPoint IdentityIQ Database – Deep Dive  

The **SailPoint Database** is the **backbone 🦴** of IdentityIQ.  
It stores everything from identities to workflows, ensuring that data survives across restarts and audits.  

---

## ⚙️ 1. Purpose of the Database  
- Stores **all configuration and operational data** for IIQ.  
- Provides **persistence** 🧠 → the server remembers who you are, what you have, and what happened before.  
- Acts as the **single source of truth** for identities, roles, policies, and history.  

---

## 📂 2. What Gets Stored in IIQ Database?  

### 🔑 Identity Data  
- **Identities** → employees, contractors, partners.  
- **Identity attributes** → name, email, department, manager.  
- **Links** → mapping between identity and accounts in applications.  

### 🌐 Applications & Accounts  
- Application definitions (AD, Workday, SAP, DB).  
- Schema (username, email, group memberships).  
- Accounts (individual user accounts inside each app).  

### 🎭 Roles & Entitlements  
- Business Roles & IT Roles.  
- Role hierarchy & policies.  
- Entitlement data (e.g., AD groups, SAP roles).  

### 🌀 Workflows & Rules  
- Workflow definitions (onboarding, certification, approvals).  
- Rule definitions (Beanshell scripts).  

### 📜 Audit & History  
- Task results (aggregation logs, provisioning history).  
- Certification campaigns & approvals.  
- Access request history.  

---

## 🧩 3. Database Schema Structure  

### Key Tables (simplified view):  
- **spt_identity** → Stores identity objects.  
- **spt_application** → Stores app configurations.  
- **spt_link** → Maps identities to accounts.  
- **spt_entitlement** → Stores entitlements.  
- **spt_task_result** → Stores task execution history.  
- **spt_workflow_case** → Tracks workflow instances.  
- **spt_audit_event** → Stores audit logs.  

👉 Every object in IIQ (Identity, Application, Role, etc.) maps to **tables** in the DB.  

---

## 🔄 4. How Database Interacts with IIQ Server  

1. **Aggregation (Inbound Sync)**  
   - Connector pulls data (e.g., from AD).  
   - Stores accounts in DB (`spt_link`, `spt_identity`).  

2. **Provisioning (Outbound)**  
   - Workflow triggers provisioning request.  
   - DB stores request → Connector executes → DB updates status.  

3. **Workflow Execution**  
   - Workflow states (pending approvals, tasks) stored in DB.  

4. **Audit & Compliance**  
   - Every action logged in DB (who approved, when, what was provisioned).  

---

## 🛠️ 5. Supported Databases  
- Oracle  
- Microsoft SQL Server  
- PostgreSQL (commonly used now)  
- MySQL (less common)  

IIQ connects to DB using **JDBC**.  

---

## 🎯 6. Developer’s Role with Database  
As a SailPoint Developer, you:  
- Rarely write queries directly (IIQ handles it).  
- But you **must understand schema** for troubleshooting.  
- Use **SQL queries** for:  
  - Debugging aggregation/provisioning issues.  
  - Validating identities, roles, entitlements.  
  - Reporting (custom extracts).  

⚡ Example Query:  
```sql
-- Find all accounts linked to a user
SELECT i.name AS IdentityName, l.application, l.nativeidentity
FROM spt_identity i
JOIN spt_link l ON i.id = l.identityid
WHERE i.name = 'Alice';
