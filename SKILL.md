---
name: frappe-native-multihand
description: Run isolated disposable Frappe benches on a native Linux host without Docker, while protecting a stable reference bench and site.
metadata:
  short-description: Native, no-Docker Frappe benches
---

# frappe-native-multihand

Use this skill when multiple developers or agents need independent Frappe benches on a host that already provides native MariaDB, Redis, Python, Node, and Bench services.

## Model

- Keep one verified reference bench unchanged.
- Create one disposable bench, site, database, Redis namespace, and port tuple per task.
- Keep the development worktree separate from the checkout used by the running bench.
- Track allocations in a locked registry so parallel runs cannot collide.
- Use SSH tunnelling for browser access; temporary servers bind to localhost only.

## Non-Docker boundary

This project intentionally does not use Docker, Docker Compose, `frappe_docker`, or container mounts. It assumes a native host and operator-provided database credentials. It does not bootstrap operating-system services or install packages.

## Required safety checks

Before mutation, verify the reference path/site and every disposable identifier are different. Refuse to operate on the reference site. Never put credentials, backups, customer data, or `.env` files in the repository. Destructive commands must target one named disposable bench and support dry-run or an explicit confirmation.

## Workflow

1. Read `docs/native-workflow.md` and set the host configuration.
2. Allocate a unique name and registry record.
3. Provision from a normal native copy or a verified seed; do not assume `bench init --clone-from` works across Bench versions.
4. Create or restore the disposable site, set unique database/Redis/port values, mute mail, pause its scheduler, and migrate only that site.
5. Overlay the requested app revisions into the bench checkout.
6. Run focused tests, API/HTTP smoke tests, and the manual browser flow. Record actual results in a test plan.
7. Stop processes, tear down only the named disposable resources, and verify the reference site remains healthy.

The shell examples are templates and require host-specific review before use.
