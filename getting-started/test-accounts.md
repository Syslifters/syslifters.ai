---
url: https://docs.syslifters.ai/getting-started/test-accounts.md
description: >-
  Why to test staging systems, how to provide test accounts and what side
  effects to expect.
---

# Test systems and accounts

## Use a staging system

::: warning We strongly recommend testing staging systems
Point syslifters.ai at a staging or test environment, not at production. The agents behave like a real attacker: they create, change and delete data, submit forms, trigger emails and password resets and may lock accounts. On a staging system with test data, none of that affects real users or real data, and no real personal data reaches the AI models.
:::

A good staging system:

* runs the same version and configuration as production;
* contains realistic test data, but no real personal data;
* can be reset or restored if the test leaves it in a bad state;
* sends emails as in production, so flows such as password reset and registration can be tested;
* has payments integrated with the payment provider's sandbox, so payment flows can be tested without real money.

If you must test production, agree on that explicitly with the system owner, use dedicated test accounts and expect the side effects listed below.

## Test accounts

Most vulnerabilities are behind the login. Provide test accounts so the agents can reach the authenticated parts of the application.

Enter them in **Test accounts** when you create the project (or later with **Edit accounts** on the **Overview** tab), one account per line:

```text
alice@example.com:Secret123!:user
bob@example.com:Secret456!:user
admin@example.com:Secret789!:admin
api-client:eyJhbGciOi...:api
```

The format is `username:password` or `username:api-token`, optionally followed by `:role`.

`role` is any short label you choose. It is not checked against a fixed list: write whatever names that account in your application, such as `user`, `admin`, `billing` or `read-only API`. Agents use the label to tell the accounts apart. Leave it off when the username is enough.

* Use **dedicated test accounts only**. Never use accounts of real users or your own personal accounts.
* Provide **at least two accounts per role**. Agents need two accounts to prove that one user can access another user's data.
* Provide accounts for **every role** the application has, including administrators.
* Agents may change the data of these accounts. They are told to keep the accounts usable, but plan for a reset after the test.

::: warning Missing administrative accounts cost time
Without administrative accounts, agents cannot see the administrative functionality. They spend the [time box](/getting-started/pentest-not-a-scan) guessing at admin endpoints instead of testing them, and authorization checks between normal users and administrators stay largely untested. The test becomes slower and less efficient. The same might apply to other missing roles.
:::

## Lockout testing

By default, agents protect the provided accounts: they do not lock them out, change their passwords or exhaust MFA attempts, and they prove missing brute-force protection with a small number of attempts only.

The **Lockout testing** switch on the **Overview** tab allows destructive authentication tests (account lockout, brute force, MFA exhaustion). Agents then still prefer throwaway accounts, but a provided account can end up locked. Only turn it on if that is acceptable.

## Account manager (optional)

If you give the agents an administrator account of the target application under **Account manager** on the **Overview** tab, they can create fresh accounts, unlock locked accounts and get throwaway accounts for destructive tests on their own. The accounts created this way are listed under **Managed accounts**.

The administrator credential is used by an agent in the application's admin interface and is processed by the AI model like any other credential.

## Side effects to expect

Tell the system owner to expect, among others:

* new, changed and deleted records created by the agents;
* emails, notifications and password reset messages triggered by the application;
* locked accounts (with lockout testing on, or after many failed logins);
* entries in logs and alerts in monitoring or intrusion detection systems;
* increased load during the run.

## Checklist

* \[ ] The target is a staging system with test data, or production testing is explicitly agreed.
* \[ ] The staging system sends emails (for example password reset) to mailboxes I can access, and payments use the provider's sandbox.
* \[ ] I provided dedicated test accounts for every role, including administrators, at least two per role, and no real user accounts.
* \[ ] Lockout testing is only on if locked accounts are acceptable.
* \[ ] The system owner knows which side effects to expect and how to reset the system afterwards.
