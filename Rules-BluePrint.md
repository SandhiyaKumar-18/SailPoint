# SailPoint Rule Writing Blueprint

In **SailPoint IdentityIQ**, a *Rule* is a piece of **Beanshell (Java-like)** code that executes at specific points in the lifecycle (provisioning, aggregation, certification, etc.).

---

## 🔹 Blueprint to Write a SailPoint Rule

```java
<Rule name="Rule_Name" type="RuleType">
   <Description>
      Short explanation of what this rule does
   </Description>
   <Source>
      <![CDATA[
         import sailpoint.object.*;
         import sailpoint.api.*;
         import java.util.*;

         // Entry point for SailPoint Rule execution
         // context = SailPointContext (provides access to IIQ objects)
         // log = SailPoint logging object

         try {
             // Example: Fetch an identity
             Identity identity = context.getObjectByName(Identity.class, "jdoe");

             if (identity != null) {
                 log.info("Identity found: " + identity.getName());
                 // Your logic goes here
             }

             // Return value depends on Rule type
             return "SUCCESS";
         } catch (Exception e) {
             log.error("Error in Rule_Name: " + e);
             throw e;
         }
      ]]>
   </Source>
</Rule>
```
# SailPoint Rules Reference

This is a quick reference for writing **SailPoint IdentityIQ Rules** in Beanshell (Java-like) syntax.

---

## 🔹 Key Components

- **Rule Name** → Unique identifier.  

- **Rule Type** → Determines when and how SailPoint runs it. Examples:  
  - `ProvisioningRule`  
  - `CorrelationRule`  
  - `BeforeProvisioningRule`  
  - `CertificationRule`  
  - `ValidationRule`  
  - `AggregationRule`  

- **Description** → Short purpose of the rule.  

- **Source** → The actual Beanshell (Java) logic inside `<![CDATA[ ... ]]>`.  

- **Context** → `context` object lets you query and modify SailPoint data.  

- **Return Value** → Depends on Rule type (e.g., a boolean, Identity, or string).  

---

## 🔹 Examples

### Simple Correlation Rule

```java
<Rule name="EmployeeIDCorrelation" type="Correlation">
   <Description>
      Correlates accounts based on EmployeeID attribute
   </Description>
   <Source>
      <![CDATA[
         String empId = account.getAttribute("employeeID");
         if (empId != null) {
             return context.getObjectByAttribute(Identity.class, "employeeID", empId);
         }
         return null;
      ]]>
   </Source>
</Rule>
```

## 🔹 Best Practices

- Always use logging (log.info) for debugging.

- Handle exceptions gracefully with try-catch.

- Keep rules modular (don’t write huge blocks of code).

- Follow naming conventions → AppName_Function_RuleType.

- Return exact data type expected by the Rule type.
