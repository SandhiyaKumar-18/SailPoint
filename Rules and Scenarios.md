# SailPoint Rules-Based Scenario Questions

This document contains practical scenario questions for **SailPoint IdentityNow / IIQ rules**. Each scenario includes the **context**, **rule type**, and **key question**.

---

## 1️⃣ ProvisioningRule Scenarios

**Scenario 1**  
- **Context:** Onboarding a new employee from HR; automatically assign AD group based on department.  
- **Question:** How would you write a provisioning rule that checks the `department` attribute and assigns the correct AD group?

**Scenario 2**  
- **Context:** Prevent contractors from being provisioned to certain applications.  
- **Question:** Which attributes would you check, and how would you enforce this restriction?

**Scenario 3**  
- **Context:** Assign default roles when a new identity is created, based on location or business unit.  
- **Question:** How would you implement this logic in a provisioning rule?

---

## 2️⃣ CorrelationRule Scenarios

**Scenario 4**  
- **Context:** Multiple source systems (HR, LDAP, Azure AD) exist. A new employee appears in HR and Azure AD.  
- **Question:** How would you design a correlation rule to merge accounts based on `email` and `employeeID` while avoiding duplicates?

**Scenario 5**  
- **Context:** Salesforce users have the same name but different emails.  
- **Question:** How would you ensure the correlation rule avoids false positives when merging accounts?

**Scenario 6**  
- **Context:** Some applications use external IDs instead of email for identity matching.  
- **Question:** How would you modify the correlation rule to support external ID matching?

---

## 3️⃣ BeforeProvisioningRule Scenarios

**Scenario 7**  
- **Context:** Usernames must follow `first.last@company.com` format before provisioning to ServiceNow.  
- **Question:** How would you implement a rule to validate usernames and reject invalid ones?

**Scenario 8**  
- **Context:** Set default manager for users based on department before provisioning.  
- **Question:** How would you implement this enrichment using a `BeforeProvisioningRule`?

**Scenario 9**  
- **Context:** Certain applications require custom attribute mapping before provisioning.  
- **Question:** How would you ensure these attributes are set correctly before account creation?

---

## 4️⃣ CertificationRule Scenarios

**Scenario 10**  
- **Context:** Managers should certify only users in their team during quarterly access certification.  
- **Question:** How would you implement a certification rule that filters certification items by `manager` attribute?

**Scenario 11**  
- **Context:** Critical applications require dual approval during certification.  
- **Question:** How can a certification rule flag users needing additional approval?

**Scenario 12**  
- **Context:** Users with privileged roles should be recertified more frequently.  
- **Question:** How would you design a rule to enforce different certification schedules?

---

## 5️⃣ ValidationRule Scenarios

**Scenario 13**  
- **Context:** No user should have both `Admin` and `Contractor` roles.  
- **Question:** How would a validation rule enforce this during provisioning?

**Scenario 14**  
- **Context:** Email addresses must always be lowercase.  
- **Question:** How would you write a validation rule to reject invalid email formatting?

**Scenario 15**  
- **Context:** Prevent assigning duplicate roles to the same user.  
- **Question:** How would you implement this check using a validation rule?

---

## 6️⃣ AggregationRule Scenarios

**Scenario 16**  
- **Context:** Fetch only active users from HR to reduce unnecessary account creation.  
- **Question:** How would you write an aggregation rule to filter inactive accounts?

**Scenario 17**  
- **Context:** Enrich imported user data with custom attributes like `OfficeLocation` from LDAP.  
- **Question:** How can an aggregation rule populate additional fields?

**Scenario 18**  
- **Context:** Remove duplicate accounts during aggregation from multiple sources.  
- **Question:** How would you implement logic to detect and ignore duplicates?

---

## 7️⃣ Advanced / Mixed Rule Scenarios

**Scenario 19**  
- **Context:** Provisioning to an application depends on approval workflow completion.  
- **Question:** How would you combine provisioning and validation rules to enforce this?

**Scenario 20**  
- **Context:** Audit requires logging all failed provisioning attempts.  
- **Question:** How would you design rules to capture and store these failures?

**Scenario 21**  
- **Context:** User accounts from terminated employees must be deprovisioned automatically.  
- **Question:** Which rule type would you use, and how would you implement the logic?

**Scenario 22**  
- **Context:** You want to trigger email notifications when a critical role is assigned.  
- **Question:** How would you implement this using rules?

**Scenario 23**  
- **Context:** Certain departments require temporary access that expires automatically.  
- **Question:** How would you implement a rule to handle temporary role assignments?

---

### ⚡ Tips for Practice
1. Identify **trigger events**: creation, update, aggregation, certification.  
2. Focus on **key attributes**: email, employeeID, department, roles, status.  
3. Define **actions** clearly: assign group, reject, enrich, log, notify.  
4. Consider **error handling** and **duplicate prevention**.  

