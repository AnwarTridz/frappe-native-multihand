# Test plan: `<name>`

Host: `<host>`  
Bench: `<disposable-bench>`  
Site: `<disposable-site>`  
Port: `<port>`

## Safety preflight

- [ ] Not the reference bench or site
- [ ] No Docker involved
- [ ] Unique database, Redis namespace, and port
- [ ] Mail muted and scheduler paused

## Results

| Area | Command/action | Result | Evidence |
|---|---|---|---|
| Unit | `<command>` | `<result>` | `<log>` |
| Site | `<command>` | `<result>` | `<log>` |
| HTTP | `<request>` | `<result>` | `<log>` |
| UI | `<manual action>` | `<result>` | `<screenshot/log>` |

## Teardown

- [ ] Temporary process stopped
- [ ] Disposable site/database removed
- [ ] Reference site still healthy
