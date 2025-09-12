# 🛡️ Correlation Rule – Detailed Guide

## 1️⃣ What is a Correlation Rule?

A **Correlation Rule** is used to **match a system account to an identity** in IdentityIQ during aggregation or reconciliation.  
It answers the question:

> “Does this account in a system belong to an existing identity in IIQ?”

---

## 2️⃣ Purpose

- Ensure **accounts are linked correctly** to identities.  
- Avoid creating **duplicate identities**.  
- Enable **accurate provisioning, role assignment, and reporting**.  
- Critical for **HR-driven onboarding/offboarding and access reviews**.

---

## 3️⃣ Inputs

Correlation rules usually receive the following objects:

| Input Object | Description |
|--------------|-------------|
| `account` | System account being processed (AD, LDAP, SaaS, etc.) |
| `identity` | Existing IdentityIQ identity object |
| `context` | `SailPointContext` object for API calls, DB queries, etc. |

---

## 4️⃣ Output / Return

- **Boolean (`true` / `false`)**  
  - `true` → the account matches the identity  
  - `false` → the account does not match  
- IIQ uses this return value to **link or ignore the account** during aggregation.

---

## 5️⃣ Common Matching Criteria

- `employeeID`  
- `email`  
- `username` / `login`  
- Custom attributes (e.g., `SAP_ID`, `BadgeNumber`)  

> Typically, you combine multiple attributes for **reliable correlation**.

---

## 6️⃣ Example – Simple EmployeeID Match

```java
// Correlate account to identity by employeeID
String acctEmpId = account.getAttribute("employeeID");
String idEmpId = identity.getAttribute("employeeID");

if(acctEmpId != null && acctEmpId.equals(idEmpId)){
    return true; // Correlation successful
}
return false;

```

## 🛡️ Correlation Rule – Scenarios Table 🚀

| Scenario | Matching Attribute(s) | Expected Outcome | Complexity / Notes |
|----------|---------------------|----------------|------------------|
| **New Employee Onboarding** | `employeeID`, optional `email` | Account linked to new identity | Basic; standard HR-driven onboarding. |
| **Employee Transfers / Department Changes** | `employeeID`, optional `department`, `manager` | Identity remains consistent; roles/entitlements updated | Medium; requires dynamic role adjustments. |
| **Contractor / Temporary Worker Accounts** | `email`, `ContractorID` or custom attribute | Contractor accounts linked without creating duplicates | Medium; may lack standard identifiers. |
| **Duplicate Account Detection** | `employeeID`, `email`, `department` | Correct account linked; duplicates avoided | High; multiple accounts per system. |
| **Termination / Offboarding** | `employeeID`, `username` | Accounts deactivated / access removed | Basic; ensures timely offboarding and reduces risk. |
| **Cross-System Correlation** | `employeeID`, `email`, fallback attributes | All system accounts linked to one identity | Advanced; centralizes identity across multiple systems. |
| **Partial Data / Missing Attributes** | Combination of available attributes | Best-effort correlation; may require manual review | Advanced; fallback rules needed for exceptions. |
| **Email Domain Variations** | `email` with different domains (e.g., contractor.company.com vs company.com) | Correctly match accounts despite domain differences | Advanced; requires string manipulation or regex. |
| **Multiple Systems with Conflicting IDs** | `employeeID`, `systemName` | Correct account linked to identity based on system context | Advanced; requires system-aware correlation logic. |

---

### 🔹 Key Notes

- Always **prioritize unique and stable attributes** like `employeeID`.  
- Use **multiple attributes** for complex scenarios to avoid duplicates.  
- Test each scenario in a **dev/test environment**.  
- Proper correlation is critical for **accurate provisioning, certifications, and compliance**.



# 🛡️ SailPoint Correlation Rule Practice Questions 🚀

---

## **10 Real-World Correlation Scenarios**

1. **Simple EmployeeID Match**  
   - Correlate an AD account to a new identity where `account.employeeID = identity.employeeID`.

2. **Email Matching**  
   - Correlate using email if `EmployeeID` is missing: `account.email.equals(identity.email)`.

3. **Multiple Accounts Handling**  
   - A user has multiple accounts in the same system. Correlate only the **active account** (attribute `status = Active`).

4. **Department-Based Correlation**  
   - Correlate accounts only if `account.department.equals(identity.department)` to prevent cross-department linking.

5. **Username Pattern Matching**  
   - Correlate accounts based on a username pattern, e.g., `identity.firstName + "." + identity.lastName`.

6. **Priority Rules**  
   - When multiple accounts match, correlate with the account having the **earliest creation date**.

7. **Temporary Accounts**  
   - Ignore temporary accounts: correlate only if `account.type != "temp"`.

8. **External System Accounts**  
   - Correlate accounts from **specific systems only**, e.g., only AD accounts, skip Workday.

9. **Logging and Debugging**  
   - Log all attempted correlations with `log.info()` and verify which accounts were linked or skipped.

10. **Advanced Multi-Attribute Correlation**  
    - Correlate using **combined criteria**: EmployeeID + Department + Email.  
    - If no match, return `false` to create a new account automatically.

---

## **Practice Tips**
- For each scenario:  
  1. Write a **Beanshell correlation rule**.  
  2. Create **dummy identities and accounts** in IIQ.  
  3. Run **aggregation → correlation**.  
  4. Observe and debug with `log.info()`.

---
# 🛡️ SailPoint Correlation Rules – Quick Reference Table 🚀

| #  | Scenario / Requirement | Sample Beanshell Logic |
|----|----------------------|-----------------------|
| 1  | **Simple EmployeeID Match**<br>Correlate AD account where `account.employeeID == identity.employeeID` | ```java String acctEmpId = account.getAttribute("employeeID"); String idEmpId = identity.getAttribute("employeeID"); if(acctEmpId != null && acctEmpId.equals(idEmpId)) return true; return false; ``` |
| 2  | **Email Matching**<br>Use email if EmployeeID missing | ```java String acctEmail = account.getAttribute("mail"); String idEmail = identity.getAttribute("email"); String idEmpId = identity.getAttribute("employeeID"); if((idEmpId==null || idEmpId.isEmpty()) && acctEmail != null && acctEmail.equalsIgnoreCase(idEmail)) return true; return false; ``` |
| 3  | **Multiple Accounts Handling**<br>Correlate only active accounts | ```java String status = account.getAttribute("status"); if(status != null && status.equalsIgnoreCase("Active")) { return true; } return false; ``` |
| 4  | **Department-Based Correlation**<br>Match department to prevent cross-linking | ```java String acctDept = account.getAttribute("department"); String idDept = identity.getAttribute("department"); if(acctDept != null && acctDept.equalsIgnoreCase(idDept)) return true; return false; ``` |
| 5  | **Username Pattern Matching**<br>Match pattern like first.last | ```java String acctName = account.getName(); String expected = identity.getAttribute("firstName")+"."+identity.getAttribute("lastName"); if(acctName != null && acctName.equalsIgnoreCase(expected)) return true; return false; ``` |
| 6  | **Priority Rules**<br>When multiple matches, pick earliest created | ```java Date acctDate = (Date)account.getAttribute("creationDate"); Date earliest = (Date)inputs.get("earliestDate"); if(earliest == null || acctDate.before(earliest)) { inputs.put("earliestDate", acctDate); return true; } return false; ``` |
| 7  | **Temporary Accounts**<br>Ignore temp accounts | ```java String type = account.getAttribute("type"); if(type != null && !type.equalsIgnoreCase("temp")) return true; return false; ``` |
| 8  | **External System Accounts Only**<br>Correlate only AD accounts | ```java String system = account.getApplication().getName(); if(system.equalsIgnoreCase("AD")) return true; return false; ``` |
| 9  | **Logging and Debugging**<br>Log all correlation attempts | ```java String msg = "Correlating "+identity.getName()+" with "+account.getName(); log.info(msg); return true; // or false based on your logic ``` |
| 10 | **Advanced Multi-Attribute Correlation**<br>Combine EmployeeID + Dept + Email | ```java String acctEmpId = account.getAttribute("employeeID"); String idEmpId = identity.getAttribute("employeeID"); String acctDept = account.getAttribute("department"); String idDept = identity.getAttribute("department"); String acctEmail = account.getAttribute("mail"); String idEmail = identity.getAttribute("email"); if(acctEmpId != null && acctEmpId.equals(idEmpId) && acctDept != null && acctDept.equalsIgnoreCase(idDept) && acctEmail != null && acctEmail.equalsIgnoreCase(idEmail)) return true; return false; ``` |

---

✅ **Tips for Practice:**  
- Paste the code into **correlation rules** in IIQ  
- Create **dummy identities and accounts** for each scenario  
- Run **aggregation → correlation**  
- Observe logs using `log.info()` to verify behavior  

---

# 🛡️ SailPoint Correlation Rules – Quick Reference Table 🚀

| #  | Scenario / Requirement | Sample Beanshell Logic |
|----|----------------------|-----------------------|
| 1  | **Simple EmployeeID Match**<br>Correlate AD account where `account.employeeID == identity.employeeID` | ```java String acctEmpId = account.getAttribute("employeeID"); String idEmpId = identity.getAttribute("employeeID"); if (acctEmpId != null && acctEmpId.equals(idEmpId)) { return true; } return false; ``` |
| 2  | **Email Matching**<br>Use email if EmployeeID missing | ```java String acctEmail = account.getAttribute("mail"); String idEmail = identity.getAttribute("email"); String idEmpId = identity.getAttribute("employeeID"); if ((idEmpId == null || idEmpId.isEmpty()) && acctEmail != null && acctEmail.equalsIgnoreCase(idEmail)) { return true; } return false; ``` |
| 3  | **Multiple Accounts Handling**<br>Correlate only active accounts | ```java String status = account.getAttribute("status"); if (status != null && status.equalsIgnoreCase("Active")) { return true; } return false; ``` |
| 4  | **Department-Based Correlation**<br>Match department to prevent cross-linking | ```java String acctDept = account.getAttribute("department"); String idDept = identity.getAttribute("department"); if (acctDept != null && acctDept.equalsIgnoreCase(idDept)) { return true; } return false; ``` |
| 5  | **Username Pattern Matching**<br>Match pattern like first.last | ```java String acctName = account.getName(); String expected = identity.getAttribute("firstName") + "." + identity.getAttribute("lastName"); if (acctName != null && acctName.equalsIgnoreCase(expected)) { return true; } return false; ``` |
| 6  | **Priority Rules**<br>When multiple matches, pick earliest created | ```java Date acctDate = (Date) account.getAttribute("creationDate"); Date earliest = (Date) inputs.get("earliestDate"); if (earliest == null || acctDate.before(earliest)) { inputs.put("earliestDate", acctDate); return true; } return false; ``` |
| 7  | **Temporary Accounts**<br>Ignore temp accounts | ```java String type = account.getAttribute("type"); if (type != null && !type.equalsIgnoreCase("temp")) { return true; } return false; ``` |
| 8  | **External System Accounts Only**<br>Correlate only AD accounts | ```java String system = account.getApplication().getName(); if (system.equalsIgnoreCase("AD")) { return true; } return false; ``` |
| 9  | **Logging and Debugging**<br>Log all correlation attempts | ```java String msg = "Correlating " + identity.getName() + " with " + account.getName(); log.info(msg); return true; // or false based on your logic ``` |
| 10 | **Advanced Multi-Attribute Correlation**<br>Combine EmployeeID + Dept + Email | ```java String acctEmpId = account.getAttribute("employeeID"); String idEmpId = identity.getAttribute("employeeID"); String acctDept = account.getAttribute("department"); String idDept = identity.getAttribute("department"); String acctEmail = account.getAttribute("mail"); String idEmail = identity.getAttribute("email"); if (acctEmpId != null && acctEmpId.equals(idEmpId) && acctDept != null && acctDept.equalsIgnoreCase(idDept) && acctEmail != null && acctEmail.equalsIgnoreCase(idEmail)) { return true; } return false; ``` |


✅ **Practice Tips:**  
- Paste the code into **correlation rules** in IIQ  
- Create **dummy identities and accounts** for each scenario  
- Run **aggregation → correlation**  
- Observe logs using `log.info()` to verify behavior  

---

This version preserves **indentation and formatting** for easy readability.  

I can also make a **fully expanded `.md` file with each scenario having multiple lines per code block** so it looks like a clean, readable script for practice.  

Do you want me to do that?



