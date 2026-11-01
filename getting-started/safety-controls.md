---
url: https://docs.syslifters.ai/getting-started/safety-controls.md
description: Permission modes, approvals, Pause and Stop hunting.
---

# Safety controls

The [scope](/legal/scope) is the hard boundary: agents can never reach an out-of-scope host. Within the scope, these controls decide how much the agents may do without asking you, and how you stop them.

## Permission modes

Every project has a **Permission mode**. It decides which agent actions need your approval.

| Mode | Behaviour | Use it when |
| --- | --- | --- |
| **Bypass approvals** | In-scope actions run without asking. | You trust the scope and want an uninterrupted run, typically on a staging system. |
| **Auto-classify actions** (default) | Agents run normal actions on their own. An action that the model's safety classifier flags as risky is held until you decide. | The usual choice: you are only asked about risky actions. |
| **Approve flagged actions** | The strictest mode: flagged actions always wait for your decision. | Sensitive targets where you want to review risky actions yourself. Expect many approval requests. |

In every mode, an action that targets an out-of-scope host is never executed silently; it becomes an approval request.

Depending on the model, **Auto-classify** can still produce many approval requests (models differ in how much they flag). If that happens on a staging system, switching to **Bypass approvals** is usually the right answer.

::: warning Bypass approvals is your decision
With **Bypass approvals**, nobody reviews the agents' actions before they run. Every in-scope action is executed, including actions that change or delete data, lock accounts or put load on the system. Choosing this mode means you accept the impact of all those actions on the target, just as if you had approved each one yourself.

Only choose it if the system owner has agreed to this kind of testing and the target can tolerate it, typically a staging system that can be reset. The scope boundary still applies: out-of-scope hosts remain unreachable.
:::

### Changing the mode during a run

You can change the mode at any time on the **Overview** tab (**Apply mode**).

* Loosening the mode (for example to **Bypass approvals**) applies immediately, also to agents that are already running.
* Tightening the mode applies to agents that start afterwards. Agents that are already running keep the looser mode until they finish. Pause and resume the project if the stricter mode must apply to everyone at once.

## Approvals

When an agent needs your decision, the workspace shows an approval request with the agent, the action and its reason.

* **Allow once** lets this single action run.
* **Allow & add host** (for a host that is out of scope) lets the action run and adds the host to the project's scope permanently. Only use it if your [authorization](/legal/authorization) covers that host.
* **Deny** blocks the action. The agent continues with something else.

An approval request that nobody answers is denied automatically after about 10 minutes. Only that single action is denied; the run continues.

::: warning You are responsible for what you allow
An approval is a decision you make on behalf of the test. When you allow an action, you take responsibility for its effect on the target, including data changes, locked accounts, triggered emails and load. When you add a host, you confirm that your [authorization](/legal/authorization) covers it.

Read the action and its reason before you decide. If you cannot judge the impact, choose **Deny**: the agent continues with other tests, and you lose at most that single action.
:::

The **Approvals** button in the workspace header shows all past decisions, including who decided and when.

## Pause and Resume

**Pause** stops all agents of the project. **Resume** continues the run where it stopped; agents pick up their work where they left off. Use Pause whenever you need the agents to stop for a while, for example when the system owner reports a problem.

## Stop hunting

**Stop hunting** ends the testing for good. Use it when the [time box](/getting-started/pentest-not-a-scan) is over.

* Hunting agents that are running or waiting are aborted.
* Every test that has not run yet is marked as skipped.
* Findings that already exist are still verified, reported and illustrated.
* The project then finishes as **completed with gaps**.

::: warning Stop hunting cannot be undone
Hunting cannot be resumed after Stop hunting. If you only want to stop for now and continue later, use **Pause** instead.
:::

## If something goes wrong on the target

If the target shows problems during a run (errors, high load, unexpected data changes):

1. Press **Pause**. All agents stop.
2. Check the **Agents** tab to see which agents were running and what they did.
3. Clarify the problem with the system owner. If needed, narrow the scope with **Edit scope** or tighten the permission mode.
4. Press **Resume** to continue, or **Stop hunting** to end the test with the findings found so far.

## Checklist

* \[ ] I chose a permission mode that fits the target (staging or production).
* \[ ] If I use **Bypass approvals**, I accept the impact of every in-scope action the agents take.
* \[ ] I only allow actions whose impact I can judge and accept.
* \[ ] I will be available to answer approval requests during the run, or I chose **Bypass approvals**.
* \[ ] I know the difference between **Pause** (resumable) and **Stop hunting** (final).
