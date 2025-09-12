# 🔗 SailPoint Connector Framework – Deep Dive  

The **Connector Framework** is the **bridge 🌉** that links SailPoint IdentityIQ with external systems.  
Without connectors, IIQ is just an island 🏝️ — connectors bring life by enabling data flow in and out.  

---

## ⚙️ 1. What is the Connector Framework?  
- A set of adapters that let IdentityIQ **communicate with external applications**.  
- Supports both directions:  
  - **Inbound (Aggregation)** → Pulls data into IIQ.  
  - **Outbound (Provisioning)** → Pushes changes to target apps.  

👉 Think of it as a **translator 🌐** between IIQ and hundreds of enterprise applications.  

---

## 🏗️ 2. Core Functions of Connectors  

1. **Aggregation (Importing Data)** 📥  
   - Collects accounts, entitlements, and groups from apps.  
   - Example: Pull all users & groups from Active Directory.  

2. **Correlation** 🔍  
   - Maps external accounts → to the correct **identity** in IIQ.  
   - Uses correlation rules like matching **employeeID** or **email**.  

3. **Provisioning (Outbound Actions)** 📤  
   - Creates, updates, disables, or deletes accounts in apps.  
   - Example: New hire in HR → IIQ creates an AD account.  

4. **Password Management** 🔑  
   - Syncs or resets passwords across systems.  
   - Example: User resets password in IIQ → updates AD + SAP.  

5. **Entitlement Management** 🎟️  
   - Manages fine-grained access (roles, groups, privileges).  
   - Example: Assign Salesforce “Sales_Manager” role to a user.  

---

## 🧩 3. Types of Connectors  

### 1. **Direct Connectors** 🚀  
- Built-in, optimized connectors for popular apps.  
- Examples: Active Directory, LDAP, DB, Workday, SAP.  

### 2. **Delimited File Connectors** 📂  
- Work with flat files (CSV, Excel).  
- Example: HR exports employee data to CSV → IIQ imports it.  

### 3. **Generic Connectors (JDBC, LDAP, Web Services)** 🌐  
- Protocol-based, flexible connectors.  
- JDBC → any DB.  
- LDAP → any directory.  
- Web Services → REST/SOAP APIs.  

### 4. **Custom Connectors** 🧑‍💻  
- Built when no standard connector exists.  
- Developers use Java + SailPoint APIs.  

---

## 🔄 4. How Connectors Work (Flow)  

1. **Application Definition**  
   - Define an application in IIQ → map schema (username, email, groups).  

2. **Aggregation**  
   - Connector reads user/group data → stores in **spt_link** & **spt_identity**.  

3. **Correlation**  
   - Rules decide which account belongs to which identity.  

4. **Provisioning Request**  
   - Workflow generates provisioning plan → Connector executes.  

5. **Response Handling**  
   - Connector reports success/failure → IIQ logs the event.  

👉 Example Flow:  
HR adds “Alice” → IIQ triggers provisioning → Connector creates AD account → Success logged → UI shows ✅.  

---

## 🛠️ 5. Connector Capabilities  

- **Full Aggregation** → Pull all accounts.  
- **Incremental Aggregation** → Pull only changes (faster).  
- **Entitlement Harvesting** → Read groups/roles from target apps.  
- **Provisioning Plans** → XML plans describing account actions.  
- **Retry Mechanisms** → Handle failures gracefully.  
- **Extensibility** → Supports **before/after rules** for custom logic.  

---

## 🎯 6. Developer’s Role with Connectors  

As a SailPoint Developer, you’ll:  
- Configure **applications & schemas**.  
- Write **Correlation Rules** (map accounts to identities).  
- Write **Provisioning Rules** (customize account creation).  
- Debug connector logs for failures.  
- Build **custom connectors** for unsupported systems.  

👉 Common Rules:  
- **Correlation Rule** – Match `employeeID`.  
- **Before Provisioning Rule** – Format username (`first.last@domain.com`).  
- **After Provisioning Rule** – Log success/failure to custom table.  

---

## 🔍 7. Real-World Analogy  

Think of connectors as **travel adapters 🔌**:  
- **IIQ = Laptop 💻**  
- **Applications = Power sockets 🔋**  
- **Connector = Adapter ⚡**  

Without the right adapter (connector), your laptop (IIQ) cannot charge (provision/aggregate).  

---

✨ **In short:**  
- **Connector Framework = bridge 🌉** between IIQ and apps.  
- **Inbound = Aggregation 📥** → bring data into IIQ.  
- **Outbound = Provisioning 📤** → send changes out.  
- **Rules 🧑‍💻** make connectors flexible & customizable.  
