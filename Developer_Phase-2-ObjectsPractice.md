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

| Method / Attribute | Purpose | Example (How to Use) |
|-------------------|--------|--------------------|
| `identity.getName()` | Returns username | `String username = identity.getName();` |
| `identity.getAttribute("department")` | Fetch a custom attribute | `String dept = identity.getAttribute("department");` |
| `identity.setAttribute("location", "Chennai")` | Set or update an attribute | `identity.setAttribute("location", "Chennai");` |
| `identity.getRoles()` | Returns list of roles | `List<Role> roles = identity.getRoles();` |
| `identity.addRole("IT_Admin")` | Assigns a role | `identity.addRole("IT_Admin");` |
| `identity.removeRole("Intern")` | Removes a role | `identity.removeRole("Intern");` |
| `identity.getAccounts()` | Get linked accounts | `List<Account> accounts = identity.getAccounts();` |
| `identity.getEntitlements()` | Returns entitlements | `List<Entitlement> ents = identity.getEntitlements();` |
| `identity.addEntitlement(entitlement)` | Assign entitlement | `identity.addEntitlement(entitlement);` |
| `identity.removeEntitlement(entitlement)` | Remove entitlement | `identity.removeEntitlement(entitlement);` |
| `identity.isEnabled()` | Check if identity is active | `if(identity.isEnabled()){ log.info("Active"); }` |
| `identity.disable()` | Disable the identity | `identity.disable();` |
| `identity.enable()` | Enable the identity | `identity.enable();` |

**Example Use Case:** Assign roles, entitlements, or trigger notifications based on identity attributes.

---

## 2️⃣ Account 💼
Represents a system account for a specific application. Linked to identities via correlation rules.

| Method / Attribute | Purpose | Example (How to Use) |
|-------------------|--------|--------------------|
| `account.getName()` | Returns the system username | `String acctName = account.getName();` |
| `account.getApplication()` | Get the system/application it belongs to | `Application app = account.getApplication();` |
| `account.getAttribute("mail")` | Fetch a specific account attribute | `String email = account.getAttribute("mail");` |
| `account.setAttribute("title", "Manager")` | Update a specific account attribute | `account.setAttribute("title", "Manager");` |
| `account.getOwner()` | Returns linked Identity | `Identity owner = account.getOwner();` |
| `account.isEnabled()` | Check if account is active | `if(account.isEnabled()){ log.info("Account active"); }` |
| `account.enable()` | Enable the account | `account.enable();` |
| `account.disable()` | Disable the account | `account.disable();` |
| `account.delete()` | Delete account from system | `account.delete();` |

**Example Use Case:** Correlate AD accounts with identities, reconcile inactive accounts.

---

## 3️⃣ SailPointContext / .context ⚡
Provides API access to IdentityIQ objects in rules/workflows. Used for querying, creating, updating, and saving objects.


| Method | Purpose | Example (How to Use) |
|--------|--------|--------------------|
| `context.getObjectByName(Identity.class, "alice.sharma")` | Fetch a user by username | `Identity alice = context.getObjectByName(Identity.class, "alice.sharma");` |
| `context.getObjects(Role.class)` | Fetch all roles in the system | `List<Role> roles = context.getObjects(Role.class);` |
| `context.getObjects(Application.class)` | Fetch all applications | `List<Application> apps = context.getObjects(Application.class);` |
| `context.getObjects(Policy.class)` | Fetch all policies | `List<Policy> policies = context.getObjects(Policy.class);` |
| `context.saveObject(identity)` | Save changes to an identity | `identity.setAttribute("location","Chennai"); context.saveObject(identity);` |
| `context.saveObject(account)` | Save account changes | `account.setAttribute("title","Manager"); context.saveObject(account);` |
| `context.commitTransaction()` | Commit DB changes | `context.commitTransaction();` |
| `context.deleteObject(identity)` | Delete an object | `context.deleteObject(alice);` |
| `context.find(Identity.class, filter)` | Query objects using a filter | `Filter filter = Filter.eq("department","HR"); List<Identity> hrUsers = context.find(Identity.class, filter);` |


**Example Use Case:** Dynamically fetch another user or role and assign it in a rule.

---

## 4️⃣ Application 💻
Represents a connected system (AD, Workday, Salesforce, etc.).

| Method / Attribute | Purpose | Example (How to Use) |
|-------------------|--------|--------------------|
| `application.getName()` | Get system/application name | `String appName = application.getName();` |
| `application.getAccounts()` | Get all accounts in the application | `List<Account> appAccounts = application.getAccounts();` |
| `application.getSchema()` | Get application schema (attributes, mappings) | `Schema appSchema = application.getSchema(); log.info(appSchema.toString());` |
| `application.getAttribute("defaultDomain")` | Get a specific application attribute | `String domain = application.getAttribute("defaultDomain");` |


**Example Use Case:** Check which users have accounts in a specific system, or configure provisioning.

---

## 5️⃣ Role 🎭
Represents a collection of entitlements/permissions.

| Method | Purpose | Example (How to Use) |
|--------|--------|--------------------|
| `role.getName()` | Get the role name | `String roleName = role.getName();` |
| `role.getEntitlements()` | Get entitlements associated with the role | `List<Entitlement> entitlements = role.getEntitlements();` |
| `role.addEntitlement(entitlement)` | Add an entitlement to the role | `role.addEntitlement(entitlement); context.saveObject(role);` |
| `role.removeEntitlement(entitlement)` | Remove an entitlement from the role | `role.removeEntitlement(entitlement); context.saveObject(role);` |
| `role.getMembers()` | Get identities assigned to the role | `List<Identity> members = role.getMembers();` |
| `role.addMember(identity)` | Assign an identity to the role | `role.addMember(alice); context.saveObject(role);` |
| `role.removeMember(identity)` | Remove an identity from the role | `role.removeMember(bob); context.saveObject(role);` |

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

| Method | Purpose | Example (How to Use) |
|--------|--------|--------------------|
| `policy.getName()` | Get the policy name | `String policyName = policy.getName();` |
| `policy.getItems()` | Get items monitored by the policy | `List<Object> items = policy.getItems();` |
| `policy.evaluate(identity)` | Check if an identity violates the policy | `boolean violation = policy.evaluate(alice);` |
| `policy.isViolated(identity)` | Returns true/false if identity violates | `if(policy.isViolated(ali

**Example Use Case:** Auto-flag high-risk access, prevent conflicting role assignments.

---

## 8️⃣ Task / TaskContext 🛠️
Represents scheduled jobs or workflow tasks. Passed to Task Rules.

| Method | Purpose | Example (How to Use) |
|--------|--------|--------------------|
| `task.getName()` | Get the task name | `String taskName = task.getName();` |
| `task.getContext()` | Get execution context | `TaskContext ctx = task.getContext();` |
| `task.getParameter("paramName")` | Fetch task parameters | `String lastRun = task.getParameter("lastRunDate");` |
| `task.run()` | Execute the task | `task.run();` |
| `task.getResult()` | Get task execution result | `String result = task.getResult(); log.info(result);` |

**Example Use Case:** Aggregation, certification campaigns, cleanup jobs, reporting.

---

## 9️⃣ CertificationCampaign ✅
Represents a certification or review campaign.

| Method | Purpose | Example (How to Use) |
|--------|--------|--------------------|
| `campaign.getName()` | Get the campaign name | `String campaignName = campaign.getName();` |
| `campaign.getItems()` | Get items to review | `List<Object> items = campaign.getItems();` |
| `campaign.getStatus()` | Get current campaign status | `String status = campaign.getStatus();` |
| `campaign.addItem(identityOrRole)` | Add an identity or role to the campaign | `campaign.addItem(alice); context.saveObject(campaign);` |

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
