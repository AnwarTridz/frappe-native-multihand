# Installation

Clone the repository on the native host or in a control workspace:

```bash
git clone https://github.com/AnwarTridz/frappe-native-multihand.git
cd frappe-native-multihand
cp examples/config.example.env .env
```

Edit `.env` with the host's native paths, reference bench/site, service connection details, and an unused port range. Keep `.env` outside version control. The helper does not install MariaDB, Redis, Bench, or operating-system packages.

For a remote host, run the helper over SSH or place the repository in the host's control workspace. Use an SSH tunnel for browser access:

```bash
ssh -N -L 8081:127.0.0.1:8081 user@host
```
