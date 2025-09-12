# 🛡️ SailPoint Workflows – Consolidation 🚀

---

## 1️⃣ What is a Workflow?
- Sequence of steps orchestrated to automate identity processes.
- Event-driven or scheduled; never contains rule code directly.
- References independent rules for logic execution.

---

## 2️⃣ Key Components
- **Workflow Definition**: Template stored in DB (`WF_DEFINITION`).
- **Workflow Steps**: Actions like aggregate, correlate, provision, notify.
- **Rules**: Independent Beanshell scripts (`Setup → Rules`) called by steps.
- **Execution Instance**: Runtime data in `WF_INSTANCE`, `WF_STEP_INSTANCE`, `WF_LOG`.
- **Inputs/Outputs**: Pass identity, accounts, or parameters between steps.

---

## 3️⃣ Workflow Triggers
| Trigger Type | How It Works | Example |
|--------------|-------------|---------|
| Manual | Admin clicks Run | Test workflow for new identity |
| Rule-based | Trigger Rule calls `WorkflowRunner.run()` | New identity triggers onboarding |
| Scheduled Task | Runs on schedule | Nightly entitlement reconciliation |
| Event / Connector | Connector detects changes | New AD account triggers aggregation/correlation |

---

## 4️⃣ Workflow & Rule Relationship
- Rules = reusable logic  
- Workflow = orchestrator calling rules  
- Steps reference rules via `<RuleRef>`  
- Rules are **not inside workflow**, only referenced.

---

## 5️⃣ Workflow Execution Flow
1. Identity created → event detected.  
2. Triggered rule runs → calls workflow.  
3. Workflow instance created (`WF_INSTANCE`).  
4. Steps executed sequentially: aggregate → correlate → provision → entitlements → notifications.  
5. Logs stored (`WF_LOG`, `WF_STEP_INSTANCE`).  
6. User provisioned in target systems (AD, Workday, Salesforce).

---

## 6️⃣ Monitoring Execution
- **UI:** Monitor → Workflows → Workflow Instances  
- Track step status: Pending, Running, Completed, Failed  
- Logs via `log.info()` from rules  
- DB Tables: `WF_INSTANCE`, `WF_STEP_INSTANCE`, `WF_LOG`

---

## 7️⃣ Developer Actions
| Component | UI Location | Developer Task |
|-----------|------------|----------------|
| Trigger Rule | Setup → Rules | Write Beanshell logic to call workflow |
| Workflow | Design → Workflows | Drag-drop steps, attach rules, configure parameters |
| Step Rules | Setup → Rules | Provisioning, correlation, notifications logic |
| Execution | Monitor → Workflow Instances | Track logs, verify role/account creation |
| Outcome | Target Systems | Verify accounts, roles, entitlements, notifications |

---

## 8️⃣ Mastery Roadmap
**Phase 1 – Foundation:** Workflow basics, structure, steps, rule references  
**Phase 2 – Rule Integration:** Attach rules to steps, debug logic with logs  
**Phase 3 – Event-Driven Workflows:** Trigger rules for automated workflow execution  
**Phase 4 – Advanced Features:** Dynamic inputs/outputs, error handling, bulk operations  
**Phase 5 – Real-World Automation:** End-to-end workflows, multi-connector integration, optimization, documentation

---

✅ **Key Takeaways**
- Workflows orchestrate steps; rules provide logic.  
- Trigger rules automate workflow execution on identity events.  
- Execution tracked in `WF_INSTANCE`, `WF_STEP_INSTANCE`, `WF_LOG`.  
- Hands-on practice and debugging are essential for mastery.
