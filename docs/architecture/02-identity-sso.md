# Identity & SSO

The self-hosted Keycloak broker is the platform protocol and claim-normalization
boundary. It is not the authority for institutional people. Production institution
deployments use College-approved institutional identity through OIDC/OAuth, with
Microsoft Entra as the expected upstream. Development, CI, demonstrations, and
standalone operation use approved local/test identities without College
credentials.

## Browser login flow

```text
User
 │
 ▼
AC application
│
▼
Self-hosted Keycloak
│
 ├── local/recovery identity
 └── production institutional Entra upstream with MFA
 │
 ▼
Commons AI Identity Service
 │
 ▼
short-lived AI session
```

## Multiple identity domains and tenants

Keycloak normalizes every upstream so the gateway never needs to know how someone
originally authenticated:

```text
Students ──────┐
Staff ─────────┤
Faculty ───────┤
Affiliates ────┤
               ▼
     Self-hosted Keycloak
               │
               ▼
       Commons AI Gateway
```

## OpenCode's SSO story

OpenCode's normal flow expects a pasted API key. Replace that with:

```text
$ ac-code login

Opening your browser...
  Keycloak → optional institution SSO → Authentication successful

Signed in as: student@example.edu
Plan: Student
Models: 8 available
```

For headless/terminal environments without a browser, use OAuth device
authorization (`ac-code login` prints a URL + short code to enter elsewhere).
Store the resulting token in the OS credential/keychain store, not a plaintext
config file. Keep tokens short-lived; OpenCode talks only to
`https://ai.<institution-domain>` after login, never to a provider directly.

## Role taxonomy

```text
student
faculty
staff
researcher
club-developer
platform-operator
administrator
guest/affiliate
```

## Institutional groups (proposed claims)

```text
AC-AI-Users
AC-AI-Faculty
AC-AI-Researchers
AC-AI-Developers
AC-AI-Platform-Admins
AC-AI-GPU-Priority
```

See `docs/governance/roles-and-identity.md` for how these map to permissions.
