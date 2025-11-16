# Alternative Deployment Methods

While GitHub Actions is the recommended deployment method, this application can also be deployed as a webhook server or run as a one-time script.

## Option 1: Webhook Server

This approach runs the app as a persistent Flask web server that responds to GitHub webhooks and runs scheduled syncs.

### Setup

1. Follow the [GitHub App setup guide](github-app-setup.md)
2. Create a `.env` file with your configuration (see backend-specific documentation)
3. Set Flask environment variables:

```env
####################
## Flask Settings ##
####################
FLASK_APP=app
FLASK_ENV=production
FLASK_RUN_PORT=5000
FLASK_RUN_HOST=0.0.0.0
```

### Running

Install dependencies:
```bash
pipenv install
```

Start the server:
```bash
pipenv run flask run --host=0.0.0.0 --port=5000
```

### Requirements

- Server must be accessible by GitHub webhooks
- Server must be kept running continuously
- Webhook URL configured in GitHub App settings

## Option 2: One-Time Script

Run the sync once and exit. Useful for testing or manual syncs.

### Setup

1. Create a `.env` file with your configuration
2. Do NOT set `FLASK_APP` environment variable

### Running

```bash
pipenv run python app.py
```

This will:
- Execute a full sync across all installed organizations
- Exit when complete
- Not start a web server
- Not respond to webhooks

### Use Cases

- Testing configuration
- Manual sync outside of schedule
- Debugging issues
- Running from cron jobs

## Docker Deployment

A Dockerfile is provided for containerized deployments.

### Building

```bash
docker build -t github-team-sync .
```

### Running

```bash
docker run -d \
  --env-file .env \
  -p 5000:5000 \
  github-team-sync
```

## Dependencies

All deployment methods require Python 3.9 and the following libraries (installed via pipenv):

- Flask
- github3.py
- python-ldap3
- APScheduler
- python-dotenv
- PyYAML
- msal
- asyncio
- okta
- onelogin
- python-keycloak
- google-api-python-client
- google-auth-oauthlib
