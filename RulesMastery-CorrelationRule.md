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

