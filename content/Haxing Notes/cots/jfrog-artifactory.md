# JFrog - Artifactory

Artifactory is a universal artifact repository manager developed by JFrog. It is designed to host, manage, and distribute software binaries and artifacts, such as application installers, container images, libraries, and configuration files. Artifactory serves as a central hub for DevOps processes, ensuring that all artifacts, dependencies, and packages are stored and managed in a secure and efficient manner.

### Resources
- [Software Supply Chain Solutions for DevOps & Security | JFrog](https://jfrog.com/)
- [pentest-hacktricks/pentesting/pentesting-web/artifactory-hacking-guide.md at master · VasjaVn/pentest-hacktricks](https://github.com/VasjaVn/pentest-hacktricks/blob/master/pentesting/pentesting-web/artifactory-hacking-guide.md)
### Default users and passwords

| Account      | Default password                               | Notes                                                                |
| :----------- | :--------------------------------------------- | :------------------------------------------------------------------- |
| admin        | password                                       | common administration account                                        |
| access-admin | password (<6.8.0) or a random value (>= 6.8.0) | used for local administration operations only                        |
| anonymous    | ’’                                             | anonymous user to retrieve packages remotely, not enabled by default |

By default, no password locking policy is in place which makes Artifactory a prime target for credential stuffing and password spraying attacks.

### Anonymous Access
If you can access the admin panel without authentication, it means that “Anonymous access” has been enabled in the administration panel, which is a common setting used to let applications retrieve artifacts without hassle but lets you, the attacker, see more than is preferable.
### Checking account rights
Sometimes, because of a misconfiguration, anonymous is allowed to deploy files to some repositories!

To check which repositories the user can deploy to, use the following request:
```
curl http://localhost:8081/artifactory/ui/repodata?deploy=true

response: {"repoList":["artifactory-build-info","example-repo-local"]}
```
### Known vulnerabilities
#### CVE-2016-10036: Arbitrary File Upload & RCE (<4.8.6)
[Details here.](https://www.exploit-db.com/exploits/44543)

This one is getting a bit old and it’s unlikely you’ll stumble on such an outdated Artifactory version. Nevertheless it’s quite effective, as it is a simple directory traversal which nets arbitrary code execution at the Tomcat level.

#### CVE-2019-9733: Authentication bypass (<6.8.6)
[Original advisory here.](https://www.ciphertechs.com/jfrog-artifactory-advisory/)

On older versions of Artifactory (up to 6.7.3), the `access-admin` account used a default password `password`.

This local account is normally forbidden to access the UI or API, but until version 6.8.6 Artifactory could be tricked into believing the request emanated locally if the `X-Forwarded-For` HTTP header was set to `127.0.0.1`.
#### CVE-2020-7931: Server-Side Template Injection (Artifactory Pro)
[Original advisory here.](https://github.com/atredispartners/advisories/blob/master/ATREDIS-2019-0006.md)

Here’s a [tool I wrote](https://github.com/gquere/CVE-2020-7931) to automate the exploitation of this vulnerability.

These are required for exploitation:
- a user with deploy (create files) and annotate (set filtered) rights
- Artifactory Pro

The vulnerability is rather simple: if a deployed resource is set to filtered it is interpreted as a Freemarker Template, which gives the attacker a SSTI attack window. 

Here are the implemented primitives:
- basic filesystem reads
- limited filesystem writes