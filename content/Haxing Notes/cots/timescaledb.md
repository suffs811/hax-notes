# TimescaleDB

### Overview: PostgreSQL-based time-series database.

##### Key Attack Surfaces:

- **PostgreSQL Misconfigurations**: Default ports, weak auth, exposed services.
- **SQL Injection**: If used in web apps.
- **Privilege Escalation**: Via stored procedures or extensions.

### Resources:
- [timescaledb GitHub](https://github.com/timescale/timescaledb)
- [Timescale Timescaledb versions and number of CVEs, vulnerabilities](https://www.cvedetails.com/version-list/26620/111330/1/Timescale-Timescaledb.html)

### Look for:
- SQL injection
- race conditions?
- default creds
- brute forceable cred
- default admin users/permissions on install

### Tools:
- `psql`
- `sqlmap`
- `pgAudit`
- `pg_stat_statements`

## **TimescaleDB Attack Checklist**
### Reconnaissance

- [ ]  Identify PostgreSQL port (default: 5432)
- [ ]  Enumerate databases and extensions
- [ ]  Check for exposed TimescaleDB endpoints
### Exploitation

- [ ]  Test for weak or default credentials
- [ ]  Attempt SQL injection via web apps or APIs
- [ ]  Abuse PostgreSQL extensions (e.g., `timescaledb_toolkit`)
- [ ]  Exploit stored procedures for privilege escalation
### Post-Exploitation

- [ ]  Dump time-series data
- [ ]  Modify or delete records
- [ ]  Create persistent backdoors via triggers or functions