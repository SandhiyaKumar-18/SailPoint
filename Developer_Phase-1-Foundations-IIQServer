
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


