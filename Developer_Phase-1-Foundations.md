

# 🧩 SailPoint IdentityIQ Architecture – Deep Dive  

## 1️⃣ Core Components of IdentityIQ  
Think of IdentityIQ as a house 🏠. Each component is a part of the house that makes it livable:  

- **IdentityIQ Server (IIQ Application)** 🖥️  
  - The main web application (Java-based, runs on Tomcat/WebSphere).  
  - Provides UI, REST APIs, and workflow engine.  

- **Database** 🗄️  
  - Stores **identities, accounts, roles, entitlements, tasks, certifications, workflow history**.  
  - Common DBs: Oracle, MS SQL, PostgreSQL.  

- **Connector Framework** 🔗  
  - Bridges SailPoint with external apps (Active Directory, SAP, Workday, etc.).  
  - Handles **aggregation (pull data in)** and **provisioning (push changes out)**.  

- **Workflow Engine** 🔄  
  - Executes business processes like onboarding, access requests, approvals, certifications.  
  - Supports customization with **XML + Beanshell scripting**.  

- **Rules & Beanshell Scripts** 🧑‍💻  
  - Java-like scripts that extend SailPoint.  
  - Used in provisioning, aggregation, validation, correlation, etc.  

- **Task Framework** ⚙️  
  - Used for scheduled or ad-hoc jobs like reconciliation, certifications, refresh tasks.  
  - Example: Daily AD aggregation task.  

---

## 2️⃣ Key Objects in IdentityIQ  
In SailPoint, everything is an **object** 🔮. As a developer, you interact with these objects a lot:  

- **Identity Object** 👤  
  - Represents a user (employee, contractor, partner).  
  - Contains attributes (name, email, manager, department, etc.).  

- **Application Object** 🌐  
  - Represents a connected system (AD, HRMS, DB).  
  - Defines schema (attributes like username, email, group membership).  

- **Account Object** 🔑  
  - Represents a user’s account on an application (e.g., AD account).  
  - Linked to Identity via **correlation rules**.  

- **Role Object** 🎭  
  - Groups entitlements/access rights together.  
  - Types: IT Role (technical), Business Role (organizational).  

- **Entitlement Object** 🎟️  
  - Lowest-level permission (e.g., AD Group = "HR_ReadOnly").  
  - Assigned via roles, policies, or direct provisioning.  

- **Policy Object** 📜  
  - Defines compliance rules (e.g., SoD = “No one should have both HR Manager + Payroll Admin”).  

---

## 3️⃣ Provisioning Engine ⚡ (The Heartbeat)  
This is where the **magic happens** → the engine that **creates, updates, disables, or deletes accounts** in connected apps.  

📌 **How it works step by step:**  
1. **Trigger** – Something starts the process (HR file, manager request, workflow).  
2. **Identity Update** – Identity object in IIQ is updated (new hire, role assigned).  
3. **Provisioning Policy** – Defines what needs to be done (create AD account, assign group).  
4. **Provisioning Request** – A request object is created → “Create Account in AD”.  
5. **Provisioning Rules** – Custom logic runs (set username format, password policy).  
6. **Connector Executes** – Connector calls AD/DB/Cloud API to provision.  
7. **Response Handling** – Success/failure returned, logged in DB.  

👉 Example:  
- HR adds new user “Alice” → IIQ creates **Identity Object** → Workflow triggers **Provisioning Policy** → Connector creates **AD account** → Response updated in IIQ.  

---

## 4️⃣ How It All Ties Together 🕸️  

Imagine:  
- **Identity** = The user 👩  
- **Application** = Systems where user needs access (AD, Salesforce, SAP) 🌐  
- **Workflow** = The process that decides when/how 👩‍⚖️  
- **Provisioning Engine** = The executor ⚡  
- **Rules** = The brains 🧠 customizing every step  
- **Database** = The memory 📚 storing all history and objects  

---

## 5️⃣ Developer’s Lens 🔍  

As a developer, your role is to:  
- Write **rules** (custom logic at various hooks).  
- Customize **workflows** (approval chains, lifecycle flows).  
- Onboard **applications** (mapping schema, correlation rules).  
- Troubleshoot **provisioning issues** (logs + debug rules).  
- Extend IIQ with **plugins and REST APIs**.  

---

✅ **Analogy:**  
IdentityIQ is like a **railway system 🚂**:  
- Tracks = Database & connectors  
- Train = Provisioning engine  
- Stations = Applications (AD, SAP, Workday)  
- Passengers = Identities & accounts  
- Signalmen = Rules (decide what/where to go)  
- Timetable = Tasks (when to run aggregations, certifications)  


---


# 🖥️ SailPoint IdentityIQ Server (IIQ Application) – Deep Dive  

The **IdentityIQ Server** is the **core engine** that runs the entire SailPoint platform.  
Think of it as the **“control tower ✈️”** managing all IAM activities like identity lifecycle, workflows, provisioning, and certifications.  

---

## ⚙️ 1. What is IdentityIQ Server?  
- A **Java-based web application** (typically deployed on Apache Tomcat or WebSphere).  
- Provides the **UI (User Interface)** 🌐, **workflow engine**, **task scheduler**, and **integration layer**.  
- It’s where all the **rules, workflows, and connectors** are executed.  

👉 Without the IdentityIQ Server, the rest of SailPoint is just **data in a database**.  
The server brings it all to life 🚀.  

---

## 🏗️ 2. Core Responsibilities of IIQ Server  
1. **User Interface (UI)** 🎨  
   - Web-based console for admins, managers, and end users.  
   - Dashboards, identity search, access request, approvals, certifications.  

2. **Workflow Engine** 🔄  
   - Executes business processes (joiner → mover → leaver).  
   - Approvals, escalations, certifications all run here.  

3. **Provisioning Engine** ⚡  
   - Sends provisioning requests to connectors.  
   - Example: Create/disable/update AD account.  

4. **Task Scheduler** ⏰  
   - Runs background jobs (aggregation, correlation, role mining).  
   - Can be scheduled (daily, weekly) or triggered manually.  

5. **Rule Execution Environment** 🧑‍💻  
   - Runs Beanshell/Java rules at different hooks.  
   - Example: Correlation rule, provisioning rule, validation rule.  

6. **REST & Web Services Layer** 🔗  
   - Exposes APIs for integration with external apps.  
   - Example: ServiceNow, custom portals, HR systems.  

---

## 🧩 3. How It Fits in the Architecture  
- **Front-end:** Provides the **UI** where users/managers interact.  
- **Middle layer:** Executes **rules + workflows + tasks**.  
- **Back-end:** Talks to the **database** (to fetch/update identity data).  
- **External layer:** Talks to **connectors** for provisioning & aggregation.  

📌 Flow Example:  
1. HR adds new hire → Data lands in IIQ DB.  
2. **IIQ Server** picks workflow → applies rules.  
3. Provisioning engine sends request → Connector → AD.  
4. Result displayed in UI + logged in DB.  

---

## 🛠️ 4. Technical Stack of IIQ Server  
- **Language:** Java (with Beanshell scripting extension).  
- **Framework:** Struts, Hibernate, Spring (legacy components).  
- **Server:** Apache Tomcat / WebSphere.  
- **DB connection:** JDBC.  
- **UI:** JSP + HTML + JavaScript.  
- **APIs:** REST, SOAP (legacy).  

---

## 🎯 5. Developer’s Role with IIQ Server  
As a **SailPoint Developer**, you’ll often:  
- Deploy and configure IIQ WAR files on Tomcat.  
- Modify XML configs (like `iiq.properties`, `log4j.properties`).  
- Write/debug **rules** and test them in server logs.  
- Troubleshoot provisioning errors from server logs.  
- Extend UI via **custom JSPs or plugins**.  
- Use IIQ REST APIs to integrate with other platforms.  

---

## 🔍 6. Real-World Analogy  
Think of the **IIQ Server** like a **restaurant kitchen 🍽️**:  
- **Menu (UI)** = What the customer sees.  
- **Chefs (Workflow engine + rules)** = Who prepare the orders.  
- **Suppliers (Connectors)** = Who bring the raw materials.  
- **Store room (Database)** = Where everything is stored.  
- **Head Chef (Provisioning engine)** = Who ensures every dish (account) is prepared and served properly.  

Without the kitchen (IIQ Server), the restaurant cannot function 🍴.  


