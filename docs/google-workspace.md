# Google Workspace Configuration

## Prerequisites

You must delegate domain-wide authority to the service account with the following scopes:
- `https://www.googleapis.com/auth/admin.directory.group.readonly`
- `https://www.googleapis.com/auth/admin.directory.group.member.readonly`
- `https://www.googleapis.com/auth/admin.directory.user.readonly`

You must provide a Google Workspace Admin account for the service account to impersonate.
It must have Admin API permissions greater or equal to the scopes listed above.

## Required Secrets

When using GitHub Actions, configure these secrets in your repository settings:

### GitHub App Credentials
- `APP_ID` - Your GitHub App ID
- `GITHUB_APP_PRIVATE_KEY` - Your GitHub App private key (entire PEM file content)
- `WEBHOOK_SECRET` - Webhook secret (can be any value when using Actions)

### Google Workspace Credentials
- `USER_DIRECTORY` - Set to `GOOGLE_WORKSPACE`
- `GOOGLE_WORKSPACE_SA_CREDS` - Service account credentials JSON file content
- `GOOGLE_WORKSPACE_ADMIN_EMAIL` - Admin email for impersonation
- `USER_SYNC_ATTRIBUTE` - Set to `username` or `email`

### If syncing by username (custom schema)
- `GOOGLE_WORKSPACE_USERNAME_CUSTOM_SCHEMA_NAME` - Custom schema name
- `GOOGLE_WORKSPACE_USERNAME_FIELD` - Custom field name

### If syncing by email (default)
- `GOOGLE_WORKSPACE_USER_MAIL_ATTRIBUTE` - Email attribute (default: `primaryEmail`)

## Optional Secrets

- `GHE_HOST` - GitHub Enterprise hostname (omit for github.com)
- `CHANGE_THRESHOLD` - Maximum number of changes allowed (default: 25)
- `OPEN_ISSUE_ON_FAILURE` - Set to `true` to create issues on failures
- `REPO_FOR_ISSUES` - Repository for failure issues (format: `owner/repo`)
- `ISSUE_ASSIGNEE` - GitHub username to assign issues to
- `TEST_MODE` - Set to `true` to preview changes without applying
- `ADD_MEMBER` - Set to `true` to add users to org if not already members
- `SYNCMAP_YML` - Custom mapping configuration (entire syncmap.yml content)
- `SYNCMAP_ONLY` - Set to `true` to sync only teams in syncmap.yml

## Sample `.env` for Local Development

```env
#########################
## GitHub App Settings ##
#########################
WEBHOOK_SECRET=development
APP_ID=12345
PRIVATE_KEY_PATH=.ssh/team-sync.pem
# Uncomment for GitHub Enterprise
#GHE_HOST=github.example.com

###############################
## Google Workspace Settings ##
###############################
USER_DIRECTORY=GOOGLE_WORKSPACE
USER_SYNC_ATTRIBUTE=username

GOOGLE_WORKSPACE_SA_CREDS_FILE=googleAuth.json
GOOGLE_WORKSPACE_ADMIN_EMAIL=admin@example.com
GOOGLE_WORKSPACE_USERNAME_CUSTOM_SCHEMA_NAME=schema-name
GOOGLE_WORKSPACE_USERNAME_FIELD=field-name

#########################
## Additional Settings ##
#########################
CHANGE_THRESHOLD=25
TEST_MODE=false
ADD_MEMBER=false
```
