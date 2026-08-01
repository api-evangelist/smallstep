---
name: Provision a Smallstep certificate authority and provisioner
description: Create a hosted X.509 authority in Smallstep, add a provisioner to issue certificates, and optionally attach an issuance webhook.
api: openapi/smallstep-openapi-original.yml
operations: [PostAuth, GetAuthorities, PostAuthorities, GetAuthority, PostAuthorityProvisioners, ListAuthorityProvisioners, GetProvisioner, PostWebhooks]
---

# Provision a Smallstep certificate authority

Use the Smallstep Platform API (`https://gateway.smallstep.com/api`, version header `X-Smallstep-Api-Version`).

## Authenticate
1. Obtain a bearer token. Either create one in the Smallstep UI (with scopes), or exchange a client
   certificate from a trusted root via `POST /auth` (`PostAuth`) / `step api token create`.
2. Send `Authorization: Bearer <token>` on every call. Responses echo `X-Request-Id` — keep it for support.

## Steps
1. `GetAuthorities` — list existing authorities to check for one you can reuse.
2. `PostAuthorities` — create a new hosted authority (name, type, admin identity). Capture the returned authority id/domain.
3. `GetAuthority` — confirm the authority is active.
4. `PostAuthorityProvisioners` — add a provisioner (e.g. `acmeProvisioner`, `oidcProvisioner`, `jwkProvisioner`, `x5cProvisioner`, or a cloud `awsProvisioner`/`azureProvisioner`/`gcpProvisioner`) that defines how certificates are requested.
5. `ListAuthorityProvisioners` / `GetProvisioner` — verify the provisioner configuration.
6. (Optional) `PostWebhooks` — attach an ENRICHING or AUTHORIZING webhook to the provisioner so step-ca calls your endpoint during issuance. Webhook bodies are HMAC-signed.

## Conventions & errors
- List endpoints paginate via the `X-Next-Cursor` response header.
- Errors are `application/json` `{ "message": ... }` (not RFC 9457); 401 = bad/absent token, 403 = missing scope, 409 = conflict, 422 = validation.
