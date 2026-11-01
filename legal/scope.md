---
url: https://docs.syslifters.ai/legal/scope.md
description: >-
  How in-scope hosts, out-of-scope hosts and seed URLs define what syslifters.ai
  agents may touch.
---

# Scope

The scope defines which systems the agents are allowed to send traffic to. It is enforced technically: every request from an agent passes through a proxy and a firewall that only let in-scope traffic through. An agent cannot reach an out-of-scope host, even if it tries.

You set the scope in three fields when you create a project:

| Field | Purpose |
| --- | --- |
| **In-scope hosts / URLs** | Everything the agents are allowed to touch. |
| **Out-of-scope hosts / URLs** | Exceptions. They always win over in-scope entries. |
| **Seed URLs** | Entry points where recon starts. They are in scope automatically. |

Write one entry per line in each field.

## In-scope entries

| You write | What is in scope |
| --- | --- |
| `app.example.com` | The whole host, on every port. |
| `app.example.com:8443` | Only that one service: the host on port 8443. |
| `https://app.example.com` | The host on port 443 (the default port of the scheme). |
| `http://app.example.com:8080/` | The host on port 8080. |
| `*.example.com` | Every subdomain of `example.com`, on every port. It does **not** include `example.com` itself; add that as a separate line. |
| `203.0.113.10` | That IP address, on every port. |

::: warning Scope is per host and port, not per path
A URL path does not limit the scope. `https://app.example.com/shop` puts **all** of `app.example.com` on port 443 into scope, including `/admin`. If parts of a host must not be tested, the host cannot be split by path; agree with the system owner that the whole host may be tested, or leave it out.
:::

## Out-of-scope entries

Out-of-scope entries always beat in-scope entries ("deny wins"). Use them to cut exceptions out of a wider scope.

| You write | What is excluded |
| --- | --- |
| `payments.example.com` | The whole host, on every port, even if `*.example.com` is in scope. |
| `app.example.com:9000` | Only port 9000 of that host. |
| `https://sso.example.com` | The **whole** host. A URL without an explicit port excludes all ports, not only 443. |

Typical exclusions are third-party services that the application uses but that you have no permission to test: payment providers, single sign-on providers, CDNs, analytics, chat widgets and email services. The agents may still see those services referenced in the application, but they cannot send traffic to them.

## Seed URLs

Seed URLs are where recon starts. Surface mapping, crawling and attack planning all begin from them, and their hosts are automatically in scope.

* For web app and API projects at least one seed URL is required. Without seed URLs, recon does not run and the hunting agents work without a map of the application.
* List the entry points completely: the login page, the main application, the API base URL, an admin area, documentation such as an OpenAPI file. A part of the application that recon never reaches is never tested.
* For mobile-only projects the uploaded app (`.apk`) is the entry point, and seed URLs are optional.

## Changing scope during a run

There are two ways the scope changes after a project was created. In both cases you are responsible for having [permission](/legal/authorization) for the new host.

### Edit scope

On the **Overview** tab, **Edit scope** lets you change the in-scope and out-of-scope lists at any time.

* The change applies immediately to agents that are already running, both additions and removals. A removed host loses access within seconds.
* Agents are told about the change the next time they continue: a new host is reported as reachable, a removed host as off limits.
* If hosts are added while the project is hunting, the already tested categories are reopened so the new host gets tested too. A paused or finished project needs **Resume** for that.

### Approving an agent's request

When an agent needs a host that is not in scope (for example an API on another subdomain it discovered), it asks you through an approval request. **Allow & add host** adds the host to the scope permanently for the whole project. **Allow once** only allows that single action. **Deny** blocks it. See [Safety controls](/getting-started/safety-controls#approvals).

## Checklist

* \[ ] Every in-scope entry is covered by my authorization, including all ports and paths of each host.
* \[ ] Third-party services (payment, SSO, CDN, analytics) are listed as out of scope.
* \[ ] Seed URLs cover every entry point of the application.
* \[ ] Wildcards (`*.example.com`) only cover subdomains I am allowed to test.
