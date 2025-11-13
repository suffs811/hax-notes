# Keycloak

Keycloak is an open-source Identity and Access Management (IAM) solution. It allows easy implementation of single sign-on for web applications and APIs.
### Resources
- [Pentesting Keycloak Part 1: Identifying Misconfiguration Using Risk Management Tools](https://csacyber.com/blog/pentesting-keycloak-part-1-identifying-misconfiguration-using-risk-management-tools)
- [Pentesting Keycloak – Part 2: Identifying Misconfiguration Using Risk Management Tools](https://csacyber.com/blog/pentesting-keycloak-part-2)

**You can tell if an app is using keycloak from:**
- Cookie names: `KEYCLOAK_LOCAL`, etc.
- URLs: `/auth/realms/realm/protocol/openid-connect/auth`
- JWT payload: `"resource_access"` and `"scope"` field names
- Page source: link references to `.../keycloak/...`

**Keycloak version can be found at the following address:**
```
GET /auth/admin/serverinfo
```
### Realms
Enumerate for realms at `/auth/realms/realm_name/`

Wordlist: https://raw.githubusercontent.com/chrislockard/api_wordlist/master/objects.txt

Just in case the client is using a custom template for the login page, hiding the registration link, we can still try to directly access the registration link, which is: 

```
/auth/realms/<realm_name>/login-actions/registration?client_id=<same_as_the_login_page>&tab_id=<same_as_the_login_form>
```
### Client IDs
Clients are entities that can request Keycloak to authenticate a user. Most often, clients are applications and services that want to use Keycloak to secure themselves and provide a single sign-on solution. Clients can also be entities that just want to request identity information or an access token so that they can securely invoke other services on the network that Keycloak secures.

When landing on a login page of a realm, the URL will be auto-filled with the default ‘client_id’ and ‘scope’ parameters, e.g.:

`/auth/realms/<realm_name>/protocol/openid-connect/auth?client_id=account-console&redirect_uri=<...>&state=<...>&response_mode=<...>&response_type=<...>&scope=openid&nonce=<...>&code_challenge=<...>&code_challenge_method=<...>`


> It is possible to identify additional client_id via fuzzing, by keeping all the other parameters with the same value.

**Common Client IDs:**
```
account
account-console
accounts
accounts-console
admin
admin-cli
broker
brokers
realm-management
realms-management
security-admin-console
```
### Scopes
When a client is registered, you must define protocol mappers and role scope mappings for that client. It is often useful to store a client scope to make creating new clients easier by sharing some common settings. This is also useful for requesting some claims or roles to be conditionally based on the value of the scope parameter. Keycloak provides the concept of a client scope for this.

**Common Scopes**
```
address
addresses
email
emails
microprofile-jwt
offline_access
phone
openid
profile
role_list
roles
role
web-origin
web-origins
```

