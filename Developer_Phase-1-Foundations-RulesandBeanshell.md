# 🧑‍💻 Rules & Beanshell Scripts in SailPoint IdentityIQ  

## 🌟 1. What are Rules?  
- **Rules** are **custom Java-based logic** written in **Beanshell** that extend SailPoint’s standard functionality.  
- Think of them as **mini-programs** embedded into IIQ to handle **special use cases**.  

👉 SailPoint provides many **rule hooks** (places where you can inject custom logic).  

---

## ⚙️ 2. What is Beanshell?  
- **Beanshell** = lightweight Java scripting language 📝.  
- 95% = Java syntax, but more flexible.  
- Used in IIQ to write custom logic without full Java compilation.  

Example Beanshell snippet 👇  

```java
// Format username rule
String first = identity.getStringAttribute("firstname");
String last = identity.getStringAttribute("lastname");
return (first + "." + last + "@company.com").toLowerCase();

```

# 🧑‍💻 Rules in SailPoint – Usage & Types  

---

## 🔹 3. Where Are Rules Used in IIQ?  

Rules can be plugged into multiple **rule hooks** inside SailPoint IdentityIQ:  

1. **Correlation Rule** 🔍  
   - Decide how an external account maps to an identity.  
   - Example: Match on `employeeID`.  

2. **Provisioning Rule** 🚀  
   - Customize how provisioning plans are executed.  
   - Example: Format usernames, generate email addresses.  

3. **Password Rule** 🔑  
   - Enforce password complexity policies.  

4. **Certification Rule** ✅  
   - Add custom logic to certification campaigns.  
   - Example: Auto-approve low-risk access, escalate high-risk.  

5. **Workflow Rule** 🔄  
   - Embedded inside workflows to add conditional logic.  

6. **Task/Report Rules** 📊  
   - Generate custom data, cleanup operations, or reporting.  

---

## 🔹 4. Types of Rules (Common Categories)  

1. **Before Provisioning Rule** 🛠️  
   - Runs *before* provisioning → modify the provisioning plan.  
   - Example: Add default AD group for all new users.  

2. **After Provisioning Rule** 📝  
   - Runs *after* provisioning → handle logging or error handling.  
   - Example: Write provisioning results to an audit table.  

3. **Correlation Rule** 🔍  
   - Used during **aggregation** to match external accounts to identities.  
   - Example: Match `employeeID` from AD with HR record.  

4. **Transformation Rule** 🎨  
   - Change values during import/export.  
   - Example: Force `toUpperCase()` for department codes.  

5. **Workflow Rule** 🔄  
   - Conditional logic **inside workflows**.  
   - Example: Auto-skip approval if requester is a department head.  

---

```xml

<Rule name="EmployeeIDCorrelationRule" type="Correlation">
  <Source>
    <![CDATA[
      // Match account.employeeID with identity.employeeID
      String acctEmpId = account.getAttribute("employeeID");
      String idEmpId = identity.getAttribute("employeeID");

      if(acctEmpId != null && acctEmpId.equals(idEmpId)) {
          return true; // Correlated!
      }
      return false;
    ]]>
  </Source>
</Rule>

```

