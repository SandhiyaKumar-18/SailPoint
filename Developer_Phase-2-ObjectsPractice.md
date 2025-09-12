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

- [ ] **Write a Beanshell rule to auto-generate username**  
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

✨ **Tip:**  
- Try **combining objects in one workflow**:  
  - Trigger: New hire (Identity) → Assign Role → Enforce Policy → Provision to Applications → Run Task (welcome email).  
- This gives you **end-to-end hands-on experience**!  
