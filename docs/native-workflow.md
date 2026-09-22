# Native workflow

## Configuration

The reference bench is read-only by policy. Set these values in `.env`:

```text
BENCH_ROOT=/srv/frappe/benches
REFERENCE_BENCH=/srv/frappe/reference-bench
REFERENCE_SITE=reference.local
REGISTRY_PATH=/srv/frappe/benches/registry.json
WEB_BASE_PORT=8081
REDIS_BASE_INDEX=4
```

Database credentials are intentionally not documented here. Supply them through the host's existing Bench configuration or a protected operator environment.

## Provisioning

Use a normal copied bench or a verified native seed. Copying must result in independent app checkouts and independent site configuration; do not mount a developer worktree into a running bench. Allocate a unique site name, database name, Redis cache/queue/socket namespaces, and web port. Restore a recent reference backup only into the disposable site.

After restore, disable outbound mail and pause the disposable scheduler. Apply app branches, run `bench migrate` against the disposable site, and capture `bench version` plus the revision of each app.

`bench init --clone-from` is optional. Bench behavior differs by version, so if it fails or reuses incompatible app state, stop and use the normal copy-and-overlay path.

## Verification

Run focused unit tests first, then site tests, then HTTP smoke checks. Start a temporary server as:

```bash
bench --site <disposable-site> serve --port <allocated-port> --noreload
```

Bind it to localhost only. Build a frontend against the temporary same-origin URL. If the native copy did not create the normal asset setup, recreate `sites/assets/<app>` links to each app's Python `public` directory and copy the reference `assets.json`/`assets-rtl.json`; without those manifests, Frappe's `/login` renderer can fail with a null bundled-asset map. Then use an SSH tunnel for manual browser verification. Record commands, counts, logs, data identifiers, and baseline failures; written tests are not evidence of execution.

## Teardown

Stop the server and verify the exact disposable path/site/database before removing anything. Drop only the disposable site/database, remove its directory, and clean only its Redis namespace. Chain dependent commands with `&&`, retain the registry record if cleanup is partial, and confirm the reference site and database are still present and healthy.
