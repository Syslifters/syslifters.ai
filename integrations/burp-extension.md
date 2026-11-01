---
url: https://docs.syslifters.ai/integrations/burp-extension.md
description: >-
  Load a finding's reproduction chain into Burp Repeater with the syslifters.ai
  Burp extension.
---

# Burp Suite extension

The Burp Suite extension loads a confirmed finding into Burp so you can reproduce and extend it by hand.

For each finding you send, it:

* opens one **Repeater** tab per request, in the order the requests were sent, named `DAST#<finding> <n>/<N> <step>` (for example `DAST#42 1/3 login`);
* adds every request with its captured response to **Organizer**, with the agent's note for that step attached;
* shows the whole chain (step, method, URL, status, note) in the extension's own **DAST** tab.

## Download and install

Download dast-burp.jar (current version 0.1.0)

Requirements: Burp Suite Professional or Community (2025.x or newer) and Java 17 or newer.

1. In Burp, open **Extensions** → **Installed** → **Add**.
2. Set **Extension type** to **Java** and select `dast-burp.jar`.
3. A new **DAST** tab appears in Burp's top bar.

## Pair Burp with a project

A pairing code connects one Burp instance to one project.

1. In syslifters.ai, open the project and go to the **Findings** tab.
2. Click **Generate code**. The code appears in the **Burp pairing code** field.
3. In Burp's **DAST** tab, fill in:
   * **DAST URL**: the address you open syslifters.ai at.
   * **Label** (optional): a name for this Burp instance, shown next to "Burp connected" in syslifters.ai.
   * **Pairing code**: paste the code, or click **Paste** to take it from the clipboard.
4. Click **Connect**. The status changes to connected, and the Findings tab shows **Burp connected**.

Burp connects out to syslifters.ai. It opens no listener, so nothing is exposed on your machine. Burp remembers the URL, label and code and reconnects automatically after a dropped connection or a restart.

::: tip Each code belongs to one project
A code only works for the project it was generated in. Generate a new code when you switch projects, and paste it into Burp.
:::

## Send a finding to Burp

Click **Burp** next to a finding on the **Findings** tab. The button is only enabled once a pairing code is set. When Burp has loaded the requests, syslifters.ai shows how many Repeater tabs were created.

What Burp receives depends on how the finding was verified:

* **Curated chain.** The verifier replays every finding cleanly and marks each step. Burp receives only the requests of that replay, grouped into named steps with the agent's notes.
* **Unfiltered capture.** If a finding has no marked replay, Burp receives the captured traffic to the affected hosts instead, capped at 50 requests. Click **Re-test** on the finding to get a curated chain.

## Repeater tab groups

Burp's extension API cannot create Repeater tab groups or put tabs into one. To keep the `DAST#…` tabs together, set **Settings** → **Tools** → **Repeater** → **Default group for new tabs** once. The shared `DAST#` prefix also keeps them next to each other.

## Without the extension

You can download the same reproduction chain as JSON, with each request and response base64-encoded:

```text
GET /api/findings/<finding-id>/burp-bundle/?download=1
```

## Troubleshooting

| Symptom | Cause and fix |
| --- | --- |
| Status shows `Closed (4404 …)` | The code is unknown, for example because it was mistyped or the project was deleted. Generate a new code. |
| Connection fails with HTTP 403 | The **DAST URL** does not match the address syslifters.ai is served at. Use exactly the URL you open in the browser. |
| Findings tab shows **Burp not connected** | Burp is not running, the extension is unloaded, or Burp uses a different code. Check the status in Burp's **DAST** tab. |
| The **Burp** button is disabled | No pairing code is set on the Findings tab, or the page lost its connection. Generate a code or reload the page. |
| Burp shows `UNFILTERED` | The finding has no marked replay. Re-test the finding. |
