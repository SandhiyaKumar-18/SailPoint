# 🛡️ Ultimate SailPoint Rules Cheat Sheet – With Advanced Rules 🚀

| Rule Type | Purpose | Typical Inputs | Output / Return | When to Use / Notes | Example | Level |
|-----------|---------|----------------|----------------|-------------------|---------|-------|
| **Correlation Rule** | Match system accounts to identities | `account`, `identity`, `context` | `true/false` | Aggregation or reconciliation; match on employeeID, email, or custom attributes. | `return account.getAttribute("employeeID").equals(identity.getAttribute("employeeID"));` | Basic |
| **Provisioning Rule** | Customize provisioning logic | `identity`, `request`, `application`, `context` | Modified `identity` or `request` | Runs during provisioning tasks; dynamic role/entitlement assignment. | `identity.addRole("HR"); context.saveObject(identity);` | Basic |
| **Certification Rule** | Determine whether items need certification | `campaign`, `item`, `identity` | `true/false` | Used in certification campaigns; custom approval logic. | `return identity.getAttribute("status").equals("Active");` | Basic |
| **Transformation Rule** | Transform attribute values | `identity`, `account`, `attributeValue` | New attribute value | Standardize data across systems (emails, phone). | `return identity.getAttribute("firstName").toLowerCase() + "." + identity.getAttribute("lastName").toLowerCase();` | Basic |
| **Password Rule** | Validate or set passwords | `identity`, `request`, `application` | Boolean or password | Enforce custom password policies. | `return request.getPassword().matches("^(?=.*[A-Z])(?=.*\\d).{8,}$");` | Basic |
| **Notification Rule** | Customize notifications and emails | `event`, `identity` | Modified email content | Alerts for missing manager, approvals, or certifications. | `event.setSubject("Manager missing for " + identity.getName());` | Basic |
| **Task Rule** | Control scheduled tasks or workflow execution | `task`, `context` | Optional output, logging | Skip or conditionally run tasks; automation purposes. | `if(task.getName().equals("DailySync")) { log.info("Running DailySync task"); }` | Advanced |
| **Aggregation Rule** | Customize how accounts are aggregated | `application`, `identity`, `account`, `context` | List of accounts | Filter or transform accounts before reconciliation. | `return account.getAttribute("status").equals("Active");` | Advanced |
| **Attribute Refresh Rule** | Update or refresh attributes | `identity`, `account`, `context` | Updated attribute values | Keep identity attributes synced with source systems. | `identity.setAttribute("department", account.getAttribute("department"));` | Advanced |
| **Role Population Rule** | Dynamically add/remove roles | `identity`, `role`, `context` | Modified roles | Auto-assign roles based on department, location, or manager. | `if(identity.getAttribute("department").equals("Finance")) identity.addRole("Finance_Analyst");` | Advanced |
| **Entitlement Population Rule** | Assign entitlements dynamically | `identity`, `entitlement`, `context` | Modified entitlements | Auto-assign permissions, groups, or system entitlements. | `identity.addEntitlement(entitlement);` | Advanced |
| **Password Validation Rule** | Validate password before provisioning | `identity`, `password`, `context` | Boolean | Enforce password rules for identity. | `return password.length() >= 8;` | Advanced |
| **Workflow Rule** | Integrate rules into workflows | `workflow`, `identity`, `context` | Modified workflow behavior | Control workflow decisions dynamically. | `if(identity.getAttribute("location").equals("India")) workflow.setVariable("region","APAC");` | Advanced |
| **Event Trigger Rule** | Customize event-driven logic | `event`, `identity`, `context` | Custom output or action | Triggered by system events like account creation, role assignment, or termination. | `if(event.getName().equals("AccountCreated")) sendNotification(identity);` | Advanced |

---

### ✅ Summary of Levels

- **Basic Rules:** Correlation, Provisioning, Certification, Transformation, Password, Notification  
- **Advanced Rules:** Task, Aggregation, Attribute Refresh, Role/Entitlement Population, Password Validation, Workflow, Event Trigger  

> Focus first on **basic rules** to gain confidence, then master **advanced rules** for complex enterprise scenarios.
