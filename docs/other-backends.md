# Alternative User Directory Backends

This application supports multiple user directory backends in addition to Google Workspace.

## Azure AD

### Required Permissions
**Authentication methods**
- [x] Service Principal

This app requires the following Azure permissions:
- `GroupMember.Read.All`
- `User.Read.All`

### Required Secrets
- `USER_DIRECTORY` - Set to `AAD`
- `AZURE_TENANT_ID` - Azure tenant ID
- `AZURE_CLIENT_ID` - Azure client ID
- `AZURE_CLIENT_SECRET` - Azure client secret
- `AZURE_APP_SCOPE` - Application scope (default: `default`)
- `AZURE_API_ENDPOINT` - API endpoint (default: `https://graph.microsoft.com/v1.0`)
- `AZURE_USERNAME_ATTRIBUTE` - Username attribute (default: `userPrincipalName`)
- `AZURE_USER_IS_UPN` - Set to `true` if username is UPN
- `AZURE_USE_TRANSITIVE_GROUP_MEMBERS` - Set to `true` to use transitive members

### Sample `.env`
```env
USER_DIRECTORY=AAD
USER_SYNC_ATTRIBUTE=username

AZURE_TENANT_ID="<tenant_id>"
AZURE_CLIENT_ID="<client_id>"
AZURE_CLIENT_SECRET="<client_secret>"
AZURE_APP_SCOPE="default"
AZURE_API_ENDPOINT="https://graph.microsoft.com/v1.0"
AZURE_USERNAME_ATTRIBUTE=userPrincipalName
AZURE_USER_IS_UPN=true
AZURE_USE_TRANSITIVE_GROUP_MEMBERS=false
```

## LDAP / Active Directory

### Sample `.env` for Active Directory
```env
USER_DIRECTORY=LDAP
USER_SYNC_ATTRIBUTE=username

LDAP_SERVER_HOST=dc1.example.com
LDAP_SERVER_PORT=389
LDAP_BASE_DN="DC=example,DC=com"
LDAP_USER_BASE_DN="CN=Users,DC=example,DC=example"
LDAP_GROUP_BASE_DN="OU=Groups,DC=example,DC=example"
LDAP_USER_FILTER="(objectClass=person)"
LDAP_USER_ATTRIBUTE=sAMAccountName
LDAP_USER_MAIL_ATTRIBUTE=mail
LDAP_GROUP_FILTER="(&(objectClass=group)(cn={group_name}))"
LDAP_GROUP_MEMBER_ATTRIBUTE=member
LDAP_BIND_USER="bind-user@example.com"
LDAP_BIND_PASSWORD="p4$$w0rd"
LDAP_SEARCH_PAGE_SIZE=1000
```

### Sample `.env` for OpenLDAP
```env
USER_DIRECTORY=LDAP
USER_SYNC_ATTRIBUTE=username

LDAP_SERVER_HOST=dc1.example.com
LDAP_SERVER_PORT=389
LDAP_BASE_DN="dc=example,dc=com"
LDAP_USER_BASE_DN="ou=People,dc=example,dc=com"
LDAP_GROUP_BASE_DN="ou=Groups,dc=example,dc=com"
LDAP_USER_FILTER="(&(objectClass=person)({ldap_user_attribute}={username}))"
LDAP_USER_ATTRIBUTE=uid
LDAP_USER_MAIL_ATTRIBUTE=mail
LDAP_GROUP_FILTER="(&(objectClass=posixGroup)(cn={group_name}))"
LDAP_GROUP_MEMBER_ATTRIBUTE=memberUid
LDAP_BIND_USER="cn=admin,dc=example,dc=com"
LDAP_BIND_PASSWORD="p4$$w0rd"
LDAP_SEARCH_PAGE_SIZE=1000
```

## Okta

### Sample `.env`
```env
USER_DIRECTORY=OKTA
USER_SYNC_ATTRIBUTE=username

OKTA_ORG_URL=https://example.okta.com
OKTA_USERNAME_ATTRIBUTE=github_username

# Token login
OKTA_ACCESS_TOKEN=asdfghkjliptojkjsj00294759

# OR OAuth login
OKTA_AUTH_METHOD=oauth
OKTA_CLIENT_ID=abcdefghijkl
OKTA_SCOPES='okta.users.read okta.groups.read'
OKTA_PRIVATE_KEY='{"kty": "RSA", ...}'
```

## OneLogin

### Sample `.env`
```env
USER_DIRECTORY=ONELOGIN
USER_SYNC_ATTRIBUTE=username

ONELOGIN_CLIENT_ID='asdafsflkjlk13q33433445wee'
ONELOGIN_CLIENT_SECRET='ca3a86f982fjjkjjkfkhls'
REGION=US
```

## Keycloak

### Required Permissions
If you have `ADMIN_FINE_GRAINED_AUTHZ` enabled, you only need the following permission for the user realm:
- `view-users`

### Sample `.env`
```env
USER_DIRECTORY=KEYCLOAK
USER_SYNC_ATTRIBUTE=username

KEYCLOAK_USERNAME=api-account
KEYCLOAK_PASSWORD=ExamplePassword
KEYCLOAK_REALM=ExampleCorp
KEYCLOAK_ADMIN_REALM=master
KEYCLOAK_USE_GITHUB_IDP=true
```

## Additional Configuration

For all backends, see [Additional Settings](additional-settings.md) for common configuration options.
