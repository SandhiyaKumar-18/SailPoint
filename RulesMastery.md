# 🛡️ SailPoint Rules Mastery Roadmap 🚀

Mastering rules in SailPoint IdentityIQ is essential for building **flexible, reusable, and powerful automation**. This roadmap will guide you from beginner to expert rule developer.

---

## 1️⃣ Understand the Basics

**Goal:** Know what rules are and why they exist.  

- **Definition:** Rules are **custom logic scripts** (usually in Beanshell) that allow IdentityIQ to behave according to your organization’s needs.  
- **Purpose:** Used in **provisioning, certification, correlation, password management, workflows, and approvals**.  
- **Key Concept:** Rules are **reusable pieces of logic**, separate from the standard IIQ processes.

**Action Steps:**  
- Read SailPoint official docs on rules.  
- Identify examples of rules in your current IIQ instance.  

---

## 2️⃣ Learn Rule Types & Inputs

| Rule Type | Purpose | Input Objects | Output / Return |
|-----------|---------|---------------|----------------|
| **Correlation Rule** | Match accounts to identities | `account`, `identity`, `context` | `true/false` |
| **Provisioning Rule** | Custom provisioning logic | `identity`, `request`, `application`, `context` | Modified `identity` or `request` |
| **Certification Rule** | Determine if items need certification | `campaign`, `item`, `identity` | `true/false` or decision |
| **Transformation Rule** | Transform attribute values | `identity`, `account`, `attributeValue` | New attribute value |
| **Password Rule** | Validate / set passwords | `identity`, `request`, `application` | Boolean or password value |
| **Notification Rule** | Customize emails/alerts | `event`, `identity` | Email content or modifications |
| **Task Rule** | Control scheduled task behavior | `task`, `context` | Optional output, logging |

**Action Steps:**  
- Open **existing rules** in your IIQ sandbox and review inputs/outputs.  
- For each type, note which objects are passed and what is returned.  

---

## 3️⃣ Master Beanshell for Rules

**Goal:** Write flexible, error-free rules.  

- **Variables, Loops, Conditionals:** Covered in your Beanshell.md  
- **Functions:** Make reusable logic for repeated checks  
- **Logging (`log`)**: Debug every rule execution  
- **Context:** Use `.context` to query/update identities, roles, accounts dynamically  

**Example – Correlation Rule:**

```java
String acctEmpId = account.getAttribute("employeeID");
String idEmpId = identity.getAttribute("employeeID");

if(acctEmpId != null && acctEmpId.equals(idEmpId)){
    log.info("Account matched for identity: " + identity.getName());
    return true;
}
return false;
```

---

## 4️⃣ Real-World Scenarios & Hands-On Practice

**Scenario Examples:**  

1. **Auto-generate username** on identity creation → Provisioning Rule  
2. **Match AD accounts to identities using employeeID** → Correlation Rule  
3. **Auto-assign roles based on department** → Provisioning/Workflow Rule  
4. **Flag expired entitlements in certification** → Certification Rule  
5. **Send custom notification if manager not set** → Notification Rule  

**Action Steps:**  
- Implement 1–2 rules per scenario in your dev sandbox.  
- Use `log.info()` extensively to see your logic in action.  
- Validate that the rule behaves correctly when triggered by aggregation, workflow, or provisioning.

---

## 5️⃣ Best Practices for Rule Mastery

- **Keep rules simple & modular** → Avoid hardcoding values  
- **Use logging for debugging** → `log.info()` to track flow  
- **Always test in dev/test environment**  
- **Document every rule** → Purpose, input, output, and scenario  
- **Reuse functions** → Avoid duplicating code  
- **Version control** → Keep track of changes and rollback if needed  

**Example – Logging for Debugging:**

```java
log.info("Starting provisioning for identity: " + identity.getName());
if(identity.getAttribute("department") == null){
    log.warn("Department not set for " + identity.getName());
}

```

## 6️⃣ Advanced Mastery – SailPoint Rules

Mastering advanced techniques ensures your rules are **efficient, reusable, and scalable**.

---

## 🔹 Key Advanced Concepts

- **Dynamic Queries:** Use `.context.find()` to query users, roles, or accounts dynamically.  
- **Custom Functions Library:** Build a reusable Beanshell functions library for common operations.  
- **Error Handling:** Always check for `null` and catch exceptions.  
- **Performance:** Minimize loops inside provisioning/correlation rules for large data sets.  
- **Integrations:** Apply rules in workflows for advanced business logic automation.


