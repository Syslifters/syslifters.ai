---
url: https://docs.syslifters.ai/legal/data-handling.md
description: >-
  Which data reaches AI models, how the data residency setting limits model
  choice, and how project data is deleted.
---

# Data residency and data handling

## Which data reaches the AI models

The agents use AI models (LLMs). Everything an agent works with is sent to the model that runs it. That includes:

* responses from the target system, including any personal or confidential data the application returns;
* the test account credentials and account manager credentials you provide;
* uploaded source code, mobile apps and reference documents;
* the project's notes, scope and findings.

Test against [staging systems with test data](/getting-started/test-accounts) wherever possible, so that no real personal data reaches a model in the first place.

## Data residency

syslifters.ai itself is hosted within the EU. The project, its evidence and the agents' working data stay on that infrastructure.

Every project has a **Data residency** setting. It limits which models the project may use, based on where each model processes data.

| Setting | Models the project may use |
| --- | --- |
| **Local only** | Only models that run on infrastructure operated by Syslifters. |
| **EU only** | Local models, plus models that process data within the EU. |
| **Unrestricted** | Any model, including providers that process data outside the EU. |

With **EU only**, syslifters.ai and every model the project uses both process data inside the EU, so all project data stays within the EU.

Choose the setting according to your agreement with the system owner and the kind of data the target holds. The setting is enforced strictly:

* The model picker greys out every model the setting does not allow.
* Before every agent starts, the model is checked against the setting. If the model is not allowed (or its data location is unknown), the agent does not start and the workspace shows an error. A run never falls back to a model outside your setting.

## Evidence

Agents' traffic to the target is recorded as evidence: requests and responses, screenshots and the agents' notes. The evidence is kept with the project so you can review and reproduce findings, and it is used for the report.

## Deleting a project

**Delete** in the project workspace removes the project and all of its data: the knowledge, findings, approvals, evidence, recorded traffic, uploaded files and the agents' working data. Deletion cannot be undone.

* Pause a running project before deleting it.
* To confirm, you type the project's name.
* A linked **SysReptor report is not deleted**. It lives on the reporting platform; delete it there if needed.

## Checklist

* \[ ] I chose the data residency setting according to my agreement with the system owner.
* \[ ] I know that syslifters.ai is hosted in the EU, and that **EU only** keeps all project data within the EU — on syslifters.ai and on the models.
* \[ ] I know that target responses, credentials and uploaded files are processed by the selected model.
* \[ ] I prefer staging systems with test data so no real personal data is processed.
* \[ ] I know that deleting the project does not delete the SysReptor report.
