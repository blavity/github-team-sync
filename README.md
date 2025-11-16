# GitHub Team Sync

Automatically synchronize GitHub teams with Google Workspace groups using GitHub Actions. This utility eliminates the need for a persistent webhook server by running as a scheduled workflow.

## Features

- **Automated Sync**: Scheduled synchronization between GitHub teams and Google Workspace groups
- **No Server Required**: Runs entirely within GitHub Actions
- **Test Mode**: Preview changes before applying them
- **Custom Mappings**: Define custom team-to-group mappings
- **Change Threshold**: Safety limit to prevent accidental bulk changes
- **Failure Notifications**: Automatic issue creation on sync failures

## Quick Start (Google Workspace)

### Prerequisites

1. **GitHub App**: Create a GitHub App with team management permissions ([detailed guide](docs/github-app-setup.md))
2. **Google Workspace Service Account**: Configure with domain-wide delegation ([detailed guide](docs/google-workspace.md))


### Google Workspace Permissions

Your Google Workspace service account needs:
- `https://www.googleapis.com/auth/admin.directory.group.readonly`
- `https://www.googleapis.com/auth/admin.directory.group.member.readonly`
- `https://www.googleapis.com/auth/admin.directory.user.readonly`

### Configuration

1. **Fork this repository** or enable GitHub Actions in your repository

2. **Configure GitHub Secrets** in your repository settings (`Settings` → `Secrets and variables` → `Actions`):

   **Required:**
   - `APP_ID` - Your GitHub App ID
   - `GITHUB_APP_PRIVATE_KEY` - Your GitHub App private key (entire PEM file)
   - `WEBHOOK_SECRET` - Any random string (not used in Actions but required)
   - `USER_DIRECTORY` - Set to `GOOGLE_WORKSPACE`
   - `GOOGLE_WORKSPACE_SA_CREDS` - Service account credentials (entire JSON file)
   - `GOOGLE_WORKSPACE_ADMIN_EMAIL` - Admin email for impersonation (e.g., `admin@example.com`)
   - `USER_SYNC_ATTRIBUTE` - Set to `username` or `email`

   **If syncing by username (requires custom schema in Google Workspace):**
   - `GOOGLE_WORKSPACE_USERNAME_CUSTOM_SCHEMA_NAME` - Your custom schema name
   - `GOOGLE_WORKSPACE_USERNAME_FIELD` - Your custom field name

   **Optional:**
   - `GHE_HOST` - GitHub Enterprise hostname (omit for GitHub.com)
   - `CHANGE_THRESHOLD` - Max changes allowed per sync (default: `25`)
   - `TEST_MODE` - Set to `true` to preview without applying changes
   - `OPEN_ISSUE_ON_FAILURE` - Set to `true` to create issues on failures
   - `REPO_FOR_ISSUES` - Where to create issues (format: `owner/repo`)
   - `ISSUE_ASSIGNEE` - GitHub username to assign issues to
   - `SYNCMAP_YML` - Custom mapping file content (optional)

3. **Enable the workflow**: The workflow (`.github/workflows/sync-teams.yml`) runs hourly by default. You can:
   - Modify the schedule in the workflow file
   - Manually trigger from the Actions tab
   - Use test mode via manual trigger to preview changes

### Manual Trigger

1. Go to the **Actions** tab in your repository
2. Select **Sync GitHub Teams** workflow
3. Click **Run workflow**
4. Choose whether to run in test mode
5. Click **Run workflow**

## How It Works

The workflow:
1. Checks out the repository
2. Sets up Python and installs dependencies
3. Creates credential files from secrets
4. Runs the sync script (`pipenv run python app.py`)
5. Cleans up sensitive files

Team synchronization:
- Matches GitHub team slugs to Google Workspace group names
- Adds users from Google groups to corresponding GitHub teams
- Removes users from GitHub teams if they're not in the Google group
- Respects the change threshold to prevent accidental bulk changes

## Custom Team Mappings

By default, teams are matched by name (e.g., GitHub team `engineering` ↔ Google group `engineering`). 

To customize mappings, add the `SYNCMAP_YML` secret with content like:

```yaml
mapping:
  - github: dev-team
    directory: engineering-all-staff
    org: my-org-name
  - github: sre-team
    directory: site-reliability-engineers
```

See [Additional Settings](docs/additional-settings.md) for more options.

## Alternative User Directories

This tool also supports:
- **LDAP / Active Directory**
- **Azure AD**
- **Okta**
- **OneLogin**
- **Keycloak**

See [Alternative Backends](docs/other-backends.md) for configuration details.

## Alternative Deployment Methods

While GitHub Actions is recommended, you can also deploy as:
- **Webhook Server**: Persistent Flask app responding to GitHub webhooks
- **One-Time Script**: Manual sync execution
- **Docker Container**: Containerized deployment

See [Alternative Deployment](docs/alternative-deployment.md) for details.

## Documentation

- [GitHub App Setup](docs/github-app-setup.md) - Creating and configuring the GitHub App
- [Google Workspace Configuration](docs/google-workspace.md) - Detailed Google Workspace setup
- [Alternative Backends](docs/other-backends.md) - LDAP, Azure AD, Okta, OneLogin, Keycloak
- [Additional Settings](docs/additional-settings.md) - Advanced configuration options
- [Alternative Deployment](docs/alternative-deployment.md) - Webhook server and script modes

## Troubleshooting

**Workflow doesn't run:**
- Check that the workflow file exists in `.github/workflows/sync-teams.yml`
- Verify GitHub Actions is enabled for your repository
- Check the Actions tab for any errors

**Sync fails:**
- Enable `TEST_MODE=true` to see what would change without applying
- Check that all required secrets are configured
- Verify Google Workspace service account has correct permissions
- Review workflow logs in the Actions tab

**Too many changes detected:**
- Increase `CHANGE_THRESHOLD` if the changes are legitimate
- Use `TEST_MODE=true` to investigate before applying

## Support

⚠️ This is free and open-source software that is supported by the open-source community, and is not included as part of GitHub's official platform support.

## Credits
This project draws much from:
- [Flask-GitHubApp](https://github.com/bradshjg/flask-githubapp)
- [github3.py](https://github.com/sigmavirus24/github3.py)
- [msal](https://github.com/AzureAD/microsoft-authentication-library-for-python)
- [okta](https://github.com/okta/okta-sdk-python)
- [ldap3](https://github.com/cannatag/ldap3)
