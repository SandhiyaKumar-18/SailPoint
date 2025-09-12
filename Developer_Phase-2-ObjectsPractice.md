# 💻 Practice with IdentityIQ Objects  

This guide helps you **practice working with core IIQ objects**: Identity, Application, Role, Policy, and Task. Think of it as your **mini-lab 🧪** to build SailPoint expertise.  

---

## 1️⃣ Identity Object 👤  

**What it is:** Represents a user in IIQ (employee, contractor, system account).  

**Practice Exercises:**  
- Create a new identity:  
  - Name: Alice Sharma  
  - EmployeeID: 1001  
  - Email: alice.sharma@company.com  
- Update an identity attribute (e.g., department = HR)  
- Delete or disable a test identity  
- Query all identities from a specific department  

**Bonus:** Write a **Beanshell rule** to auto-generate username as `first.last@company.com`  



---

```java
// Beanshell Rule: Auto-generate username as first.last@company.com

// Get first and last name attributes from the identity
String firstName = identity.getStringAttribute("firstname");
String lastName = identity.getStringAttribute("lastname");

// Make sure the attributes are not null
if(firstName == null) firstName = "";
if(lastName == null) lastName = "";

// Generate username in lowercase: first.last@company.com
String username = (firstName + "." + lastName + "@company.com").toLowerCase();

// Return the generated username
return username;


```

---

## 2️⃣ Application Object 💼  

**What it is:** Represents a connected system (AD, SAP, Workday).  

**Practice Exercises:**  
- Add a new application: TestAD  
  - Type: Active Directory  
  - Define schema: username, email, group  
- Perform aggregation: pull all accounts from TestAD  
- Test provisioning: create a test user in TestAD from IIQ  
- Write a **correlation rule** to match accounts by employeeID  

**Bonus:** Try a **Delimited File Connector** to import test users from CSV  

---

## 3️⃣ Role Object 🎭  

**What it is:** Represents a collection of entitlements/permissions.  

**Practice Exercises:**  
- Create a role: HR_Manager  
  - Entitlements: AD_HR_Group, SAP_HR_Role  
- Assign role to a test identity (Alice Sharma)  
- Remove entitlements and see impact on the user  
- Test **role-based provisioning**  

**Bonus:** Use **dynamic roles** based on attributes (department = HR → auto-assign HR_Manager)  

---

## 4️⃣ Policy Object 📜  

**What it is:** Defines rules to enforce compliance (SoD, access restrictions).  

**Practice Exercises:**  
- Create a Segregation of Duties (SoD) policy: “Finance cannot approve their own expenses”  
- Assign policy to roles and identities  
- Test policy violations by assigning conflicting roles  
- Generate a policy violation report  

**Bonus:** Write a **policy rule** to auto-flag high-risk access  

---

## 5️⃣ Task Object 🛠️  

**What it is:** Automates background jobs (aggregation, reports, cleanups).  

**Practice Exercises:**  
- Create a TaskDefinition: “Disable inactive accounts”  
  - Frequency: daily at 2 AM  
  - Executor: Java/Beanshell class  
- Run the task manually and check TaskResult logs  
- Create a report task: “Export all HR users to CSV”  
- Schedule a task to refresh identity cubes nightly  

**Bonus:** Write a **custom cleanup task** in Beanshell  

---

## 🔍 Real-World Analogy  

- **Identity = Employee 👤**  
- **Application = Software/system 💻**  
- **Role = Job responsibilities 🎭**  
- **Policy = Company rules 📜**  
- **Task = Night-shift robot 🛠️**  

Together, they make **IdentityIQ run smoothly like a well-oiled company 🚀**  

---
## 🛡️ SailPoint Core Objects Cheat Sheet – Extended Version 🚀

In SailPoint rules and workflows, certain objects are passed as inputs.  
These objects give you access to user data, accounts, applications, context, and policies.

---

## 1️⃣ Identity 👤
Represents a user in IdentityIQ. Core object for users, roles, entitlements, and attributes.

| Method / Attribute | Purpose |
|-------------------|--------|
| `identity.getName()` | Returns username |
| `identity.getAttribute("attributeName")` | Fetch a custom attribute |
| `identity.setAttribute("attributeName", value)` | Set or update an attribute |
| `identity.getRoles()` | Returns a list of roles assigned to the user |
| `identity.addRole("RoleName")` | Assign a role |
| `identity.removeRole("RoleName")` | Remove a role |
| `identity.getAccounts()` | Get linked accounts (AD, LDAP, etc.) |
| `identity.getEntitlements()` | Returns assigned entitlements |
| `identity.addEntitlement(entitlement)` | Assign entitlement to user |
| `identity.removeEntitlement(entitlement)` | Remove entitlement from user |
| `identity.isEnabled()` | Check if identity is active |
| `identity.disable()` | Disable the identity |
| `identity.enable()` | Enable the identity |

**Example Use Case:** Assign roles, entitlements, or trigger notifications based on identity attributes.

---

## 2️⃣ Account 💼
Represents a system account for a specific application. Linked to identities via correlation rules.

| Method / Attribute | Purpose |
|-------------------|--------|
| `account.getName()` | Username in the system |
| `account.getApplication()` | System/application the account belongs to |
| `account.getAttribute("attributeName")` | Fetch account attribute |
| `account.setAttribute("attributeName", value)` | Update account attribute |
| `account.getOwner()` | Returns the linked identity (if correlated) |
| `account.isEnabled()` | Check if account is active |
| `account.enable()` | Enable account |
| `account.disable()` | Disable account |
| `account.delete()` | Delete account from system |

**Example Use Case:** Correlate AD accounts with identities, reconcile inactive accounts.

---

## 3️⃣ SailPointContext / .context ⚡
Provides API access to IdentityIQ objects in rules/workflows. Used for querying, creating, updating, and saving objects.

| Method | Purpose |
|--------|--------|
| `context.getObjectByName(Identity.class, "username")` | Fetch identity by username |
| `context.getObjects(Role.class)` | Fetch all roles |
| `context.getObjects(Application.class)` | Fetch all applications |
| `context.getObjects(Policy.class)` | Fetch all policies |
| `context.saveObject(identity)` | Save changes to an identity |
| `context.saveObject(account)` | Save account changes |
| `context.commitTransaction()` | Commit DB changes |
| `context.deleteObject(identity)` | Delete an object |
| `context.find(Identity.class, filter)` | Query objects using filters |

**Example Use Case:** Dynamically fetch another user or role and assign it in a rule.

---

## 4️⃣ Application 💻
Represents a connected system (AD, Workday, Salesforce, etc.).

| Method | Purpose |
|--------|--------|
| `application.getName()` | System name |
| `application.getAccounts()` | List all accounts in the app |
| `application.getSchema()` | Return application schema (attributes, identity mapping) |
| `application.getAttribute("attributeName")` | Get application-specific attribute |

**Example Use Case:** Check which users have accounts in a specific system, or configure provisioning.

---

## 5️⃣ Role 🎭
Represents a collection of entitlements/permissions.

| Method | Purpose |
|--------|--------|
| `role.getName()` | Role name |
| `role.getEntitlements()` | List entitlements associated with role |
| `role.addEntitlement(entitlement)` | Add entitlement to role |
| `role.removeEntitlement(entitlement)` | Remove entitlement from role |
| `role.getMembers()` | Identities assigned to the role |
| `role.addMember(identity)` | Assign identity to role |
| `role.removeMember(identity)` | Remove identity from role |

**Example Use Case:** Assign entitlements dynamically, manage role memberships.

---

## 6️⃣ Entitlement 🎯
Represents a permission, group, or system access right.

| Method | Purpose |
|--------|--------|
| `entitlement.getName()` | Entitlement name |
| `entitlement.getApplication()` | System it belongs to |
| `identity.addEntitlement(entitlement)` | Assign entitlement to a user |
| `identity.removeEntitlement(entitlement)` | Remove entitlement from a user |

**Example Use Case:** Assign access rights to identities, manage permissions in provisioning.

---

## 7️⃣ Policy 📜
Defines compliance rules such as SoD (Segregation of Duties) or access restrictions.

| Method | Purpose |
|--------|--------|
| `policy.getName()` | Policy name |
| `policy.getItems()` | Items monitored by policy |
| `policy.evaluate(identity)` | Check if identity violates policy |
| `policy.isViolated(identity)` | Returns true/false for violation |

**Example Use Case:** Auto-flag high-risk access, prevent conflicting role assignments.

---

## 8️⃣ Task / TaskContext 🛠️
Represents scheduled jobs or workflow tasks. Passed to Task Rules.

| Method | Purpose |
|--------|--------|
| `task.getName()` | Task name |
| `task.getContext()` | Execution context |
| `task.getParameter("paramName")` | Fetch task parameters |
| `task.run()` | Execute the task programmatically |
| `task.getResult()` | Returns task execution results |

**Example Use Case:** Aggregation, certification campaigns, cleanup jobs, reporting.

---

## 9️⃣ CertificationCampaign ✅
Represents a certification or review campaign.

| Method | Purpose |
|--------|--------|
| `campaign.getName()` | Campaign name |
| `campaign.getItems()` | Items to review (users/roles) |
| `campaign.getStatus()` | Current campaign status |
| `campaign.addItem(identityOrRole)` | Add an item to the campaign |

**Example Use Case:** Manage access reviews, approvals, and certification logic.

---

## 🔹 Quick Tips for Development / Interviews
- **.identity** → Core object, represents users.  
- **.account** → Use for system-level operations.  
- **.context / SailPointContext** → Query, create, update, and save objects.  
- **.application** → Understand connected systems.  
- **.role & .entitlement** → Permissions, access, and assignments.  
- **.policy** → Compliance enforcement.  
- **.task** → Scheduled automation jobs.  
- Always check **nulls** before accessing attributes.  
- Know **object interactions**: Identity → Accounts → Roles → Entitlements → Applications → Policies.  
- Methods vary by **rule type**:  
  - ProvisioningRule → Identity + Context  
  - CorrelationRule → Account + Context  

---

💡 Tip: Understanding these objects and their **key methods** is essential to **writing effective rules, workflows, and tasks** in SailPoint.  

✨ **Tip:**  
- Try **combining objects in one workflow**:  
  - Trigger: New hire (Identity) → Assign Role → Enforce Policy → Provision to Applications → Run Task (welcome email).  
- This gives you **end-to-end hands-on experience**!  
