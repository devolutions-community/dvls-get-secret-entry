# Devolutions Server Get SecretEntry Action

This GitHub Action allows you to authenticate and retrieve a secret entry from Devolutions Server.

## Prerequisites

Before using this action, you must first obtain a token using the `dvls-login` action:

```yaml
steps:
  - name: Login to Devolutions Server
    uses: devolutions/dvls-login@v1
    with:
      server_url: 'https://your-server.devolutions.app'
      app_key: ${{ secrets.DVLS_APP_KEY }}
      app_secret: ${{ secrets.DVLS_APP_SECRET }}
      # The token will be stored in DVLS_TOKEN by default
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `server_url` | URL of the Devolutions Server | Yes | - |
| `token` | Token for authentication | Yes | - |
| `vault_name` | Name of the vault containing the secret entry | Yes | - |
| `entry_name` | Name of the secret entry to retrieve | Yes | - |
| `output_variable` | Name of the environment variable to store the retrieved secret | No | `DVLS_ENTRY_SECRET` |

## Usage

```yaml
steps:
  - name: Get Secret from Devolutions Server
    uses: devolutions/dvls-get-secret-entry@v1
    with:
      server_url: 'https://your-server.devolutions.app'
      token: ${{ secrets.DVLS_TOKEN }}
      vault_name: 'MyVault'
      entry_name: 'MySecret'
      output_variable: 'MY_SECRET' # Optional, defaults to DVLS_ENTRY_SECRET
```

## Example Workflow

Here's a complete example of how to use this action in your workflow:

```yaml
name: Example Workflow
on: [push]

jobs:
  example:
    runs-on: ubuntu-latest
    steps:
      - name: Login to Devolutions Server
        uses: devolutions/dvls-login@v1
        with:
          server_url: 'https://your-server.devolutions.app'
          app_key: ${{ secrets.DVLS_APP_KEY }}
          app_secret: ${{ secrets.DVLS_APP_SECRET }}

      - name: Get Secret
        uses: devolutions/dvls-get-secret-entry@v1
        with:
          server_url: 'https://your-server.devolutions.app'
          token: ${{ env.DVLS_TOKEN }}  # Uses the token from the login step
          vault_name: 'MyVault'
          entry_name: 'MySecret'

      - name: Use Secret
        run: |
          echo "Secret is stored in ${{ env.DVLS_ENTRY_SECRET }}"
          # Your code that uses the secret
```

## Security Notes

- Store your Devolutions Server token as a GitHub Secret
- Never print secrets in logs or expose them in any way
- The secret will be stored in an environment variable that is accessible to subsequent steps in your workflow

## License

This GitHub Action is available under the [MIT License](LICENSE).
