# Creating the GitHub App

## Setup Instructions

1. On your GitHub instance, visit the `settings` page on the organization that you want to own the **GitHub** App, and navigate to the `GitHub Apps` section.
    - You can access this page by visiting the following url:
      `https://<MY_GITHUB_HOSTNAME>/organizations/<MY_ORG_NAME>/settings/apps`

2. Create a new **GitHub App** with the following settings:
    - **Webhook URL**: URL of the machine on which this app has been deployed (Example: `http://ip.of.machine:3000`) - Not required for GitHub Actions deployment
    - **Homepage URL**: URL of the machine on which this app has been deployed (Example: `http://ip.of.machine:3000`)
    - **Webhook Secret**: The webhook secret that will be or has been defined as an environment variable in your deployment environment as `WEBHOOK_SECRET`
    - **Permissions and Events**: This application will need to be able to manage teams on GitHub, so the `events` and `permissions` listed below will be required. For more information on how to create a GitHub App, please visit [https://developer.github.com/apps/building-github-apps/creating-a-github-app](https://developer.github.com/apps/building-github-apps/creating-a-github-app)

3. Once these have been configured, select the `Create GitHub App` button at the bottom of the page to continue

4. Make a note of the `APP ID` on your newly-created **GitHub App**. You will need to set this as an environment variable when configuring the app.

5. Generate and download a private key from the new App page, and store it in your deployment environment. You can either do this by saving the file directly in the environment and specifying its path with the environment variable `PRIVATE_KEY_PATH`

6. After you have created the **GitHub** App, you will need to install it to the desired **GitHub** Organizations.
    - Select `Install App`
    - Select `All Repositories` or the desired repositories you wish to watch

## Permissions and Events

### Permissions

| Category | Attribute | Permission |
| --- | --- | --- |
| Repository permissions | `Issues` | `Read & write` |
| Repository permissions | `Metadata` | `Read-only` |
| Organization permissions | `Members` | `Read & write` |
| User permissions | `Email addresses` | `Read-only` |

### Events

| Event | Required? | Description |
| --- | --- | --- |
| `Team` | Optional | Trigger when a new team is `created`, `deleted`, `edited`, `renamed`, etc. |
