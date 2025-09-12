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
