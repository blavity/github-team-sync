# Additional Settings

These settings apply to all user directory backends.

## Common Settings

### Change Threshold
```env
CHANGE_THRESHOLD=25
```
Stop if number of changes exceeds this number. Default: 25

### Failure Notifications
```env
OPEN_ISSUE_ON_FAILURE=true
REPO_FOR_ISSUES=github-demo/demo-repo
ISSUE_ASSIGNEE=githubber
```
Create an issue if the sync fails for any reason.

### Sync Schedule
```env
SYNC_SCHEDULE=0 * * * *
```
Sync schedule, cron style schedule. Default (hourly): `0 * * * *`

Note: When using GitHub Actions, the schedule is controlled by the workflow file, not this environment variable.

### Test Mode
```env
TEST_MODE=false
```
Show the changes, but do not make any changes. Default: false

### Organization Membership
```env
ADD_MEMBER=false
REMOVE_ORG_MEMBERS_WITHOUT_TEAM=false
```
- `ADD_MEMBER`: Automatically add users missing from the organization
- `REMOVE_ORG_MEMBERS_WITHOUT_TEAM`: Automatically remove users from the organization that are not part of a team

### Custom Team Mapping
```env
SYNCMAP_ONLY=false
```
Set to `true` to sync only teams defined in `syncmap.yml`.

### EMU Shortcode
```env
EMU_SHORTCODE=volcano
```
Enterprise Managed Users shortcode (if applicable).

## Custom Team/Group Mapping

Create a `syncmap.yml` file to define custom mappings between GitHub teams and directory groups:

```yaml
---
mapping:
  - github: demo-team
    directory: ldap super users
    org: my github org
  - github: demo-admin-2
    directory: some other group

# Only sync groups with matching prefixes (optional)
#group_prefix:
#  - TEST-
#  - DEMO-

# Ignore specific users (optional)
ignore_users:
  - userA
  - userB
```

The custom map uses slugs that are lowercase. If you don't specify organization name, it will synchronize all teams with same name in any organization.

When using GitHub Actions, you can provide the entire contents of `syncmap.yml` as the `SYNCMAP_YML` secret.
