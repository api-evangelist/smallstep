---
name: Search, inspect, and revoke Smallstep certificates
description: Query issued certificates in Smallstep, inspect a certificate by serial number, and revoke a compromised certificate.
api: openapi/smallstep-openapi-original.yml
operations: [PostAuth, ListCertificates, SearchCertificates, GetCertificate, RevokeCertificate]
---

# Search, inspect, and revoke certificates

Base URL `https://gateway.smallstep.com/api`; bearer auth.

## Steps
1. Authenticate (`PostAuth` / UI token / `step api token create`).
2. `ListCertificates` — list issued certificates (paginate via `X-Next-Cursor`).
3. `SearchCertificates` — filter certificates (e.g. by `deviceID`) to find the one(s) you need.
4. `GetCertificate` — fetch full detail for a specific `serialNumber`.
5. `RevokeCertificate` — revoke by `serialNumber` when a device or key is compromised.

## Notes
- Certificates are addressed by `serialNumber`.
- Revocation is irreversible — confirm the correct serial via `GetCertificate` first.
- Errors are `application/json` `{ "message": ... }`; `404` = unknown serial, `403` = missing scope.
