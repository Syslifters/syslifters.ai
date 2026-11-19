---
url: https://docs.syslifters.ai/getting-started/quickstart.md
description: Create and run your first syslifters.ai project.
---

# Quickstart

This guide takes you from an empty project list to a report. Before you start, read the pages under [Before you start](/#before-you-start); they cover your responsibilities and how to avoid unwanted impact on the target.

## 1. Sign in

Open syslifters.ai in your browser and sign in with your company account. You land on the **Projects** list.

## 2. Create a project

Click **New project** and fill in the form.

### Basics

| Field | What to enter |
| --- | --- |
| **Project name** | A name you recognise, for example "Customer portal staging". |
| **Data residency** | Which models may process the project's data. See [Data residency](/legal/data-handling#data-residency). |
| **Permission mode** | How much the agents may do without asking you. **Auto-classify actions** is a good default. See [Permission modes](/getting-started/safety-controls#permission-modes). |
| **Model** | The AI model the agents use. Models that your data residency setting does not allow are greyed out. |
| **Reasoning effort** | Higher is more thorough, but slower and more expensive. Leave the default if unsure. |
| **Target types** | **Web app**, **API** and/or **Mobile (Android)**. |

### Scope and entry points

| Field | What to enter |
| --- | --- |
| **In-scope hosts / URLs** | Every host the agents may test, one per line. |
| **Out-of-scope hosts / URLs** | Hosts the agents can never reach. |
| **Seed URLs** | The entry points where recon starts: login page, application, API base URL. |

If a login provider is out of scope, login requests to it never succeed and the agents cannot log in. If a payment provider is out of scope, payment requests to it never succeed.

See [Scope](/legal/scope) for the exact syntax and how wildcards, ports and paths behave.

### Accounts and context

| Field | What to enter |
| --- | --- |
| **Test accounts** | `username:password` or `username:api-token`, optionally `:role`, one per line. `role` is any label you choose (for example `user` or `admin`), not a fixed keyword. Accounts for every role, including administrators, and at least two per role. See [Test accounts](/getting-started/test-accounts#test-accounts). |
| **Notes** | Anything the agents should know: constraints, special flows, known quirks. |
| **Source ZIP / mobile app** | Optional. A ZIP of the application's source code helps the agents find vulnerabilities faster. For mobile projects, upload the `.apk`. |
| **Reference docs** | Optional. API specifications (for example OpenAPI files), user manuals or other documents. Agents can read them but never change them. |
| **Create a SysReptor project automatically** | Turn it on to have the report project created for you. Recommended. |

Remember that uploaded source code and documents are processed by the selected model. See [Data handling](/legal/data-handling).

Click **Create project**. The project opens in its workspace.

## 3. Start the run

Check the **Overview** tab: scope, seed URLs and accounts should be what you expect. Then click **Start run**.

The run goes through recon, hunting, verification and reporting. You can follow it in the workspace:

* **Overview**: the current phase and the project's settings.
* **Knowledge**: the documentation of the application the agents build during recon.
* **Coverage**: which test categories have been run.
* **Findings**: suspected and confirmed vulnerabilities.
* **Agents**: every agent, its status and what it is doing.
* **Cost**: the model cost of the run.

## 4. Answer approval requests

Depending on the permission mode, agents ask for your approval before risky actions or before touching a host that is not in scope. Answer with **Allow once**, **Allow & add host** or **Deny**. Unanswered requests are denied after about 10 minutes. See [Approvals](/getting-started/safety-controls#approvals).

## 5. Review findings

Suspected vulnerabilities appear in the **Findings** tab as soon as an agent reports them. Each one is then reproduced by an independent agent from a clean session. Only reproduced vulnerabilities become confirmed findings, with the evidence (requests, responses, screenshots) attached.

## 6. End the run

The run either finishes on its own or you end it when your time box is over:

* **Pause** stops all agents; **Resume** continues later.
* **Stop hunting** ends testing for good. Existing findings are still verified and reported. See [Stop hunting](/getting-started/safety-controls#stop-hunting).

## 7. Report

If the project has a SysReptor project, confirmed findings are written into it automatically, including screenshots. If you linked SysReptor after findings were already confirmed, use **Report to SysReptor** in the **Findings** tab to push them. Review and finalise the report in SysReptor.
