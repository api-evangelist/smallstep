---
name: Manage the Smallstep device inventory
description: Register devices in Smallstep, read and update their metadata, transition device lifecycle state, and remove devices.
api: openapi/smallstep-openapi-original.yml
operations: [PostAuth, ListDevices, PostDevices, GetDevice, PatchDevice, PatchDeviceLifecycle, DeleteDevice]
---

# Manage the Smallstep device inventory

Base URL `https://gateway.smallstep.com/api`; bearer auth (`Authorization: Bearer <token>`).

## Steps
1. Authenticate (`PostAuth` / UI token / `step api token create`).
2. `ListDevices` — enumerate the current inventory; page with the `X-Next-Cursor` header.
3. `PostDevices` — register a device (serial, OS, ownership, user, tags — see `deviceRequest`).
4. `GetDevice` — read a single device by id.
5. `PatchDevice` — update mutable device metadata (`devicePatch`: display name, tags, owner).
6. `PatchDeviceLifecycle` — transition lifecycle status (`deviceLifecyclePatch`, e.g. enroll/retire) without deleting the record.
7. `DeleteDevice` — permanently remove a device from inventory.

## Notes
- Device IDs are UUIDs; a `422` means the body failed validation, `404` an unknown id.
- Prefer `PatchDeviceLifecycle` over `DeleteDevice` when retiring hardware you may re-enroll.
- Every response carries `X-Request-Id`; quote it to support.
