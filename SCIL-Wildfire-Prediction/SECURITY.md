# Security and API keys

## Never commit credentials

The notebooks support private values through prompts or environment variables:

- `FIRMS_MAP_KEY`
- `NGC_API_KEY` or `NGC_CLI_API_KEY`
- `CARTO_API_KEY`
- `CORRDIFF_NIM_URL` for the service address

Do not place the actual values in a notebook, README, shell script, saved terminal transcript, or `.env` file tracked by Git.

Example for a temporary Bash session:

```bash
export FIRMS_MAP_KEY="your-key"
export NGC_API_KEY="your-key"
export CORRDIFF_NIM_URL="http://localhost:8000"
```

Example for a temporary Windows PowerShell session:

```powershell
$env:FIRMS_MAP_KEY = "your-key"
$env:NGC_API_KEY = "your-key"
$env:CORRDIFF_NIM_URL = "http://localhost:8000"
```

## If a key was committed

1. Revoke or rotate the key immediately.
2. Replace the code with an environment-variable lookup or secure prompt.
3. Remove the secret from Git history if required by the repository's security policy.
4. Treat the old key as compromised even if the repository is private or the latest file no longer shows it.

GitHub documents the history-rewrite process in [Removing sensitive data from a repository](https://docs.github.com/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository).

## Generated HTML warning

The CONUS notebook inserts the optional CARTO key into the tile URL. As a result, an HTML map generated with that key is not safe to publish. Leave the CARTO prompt blank and use OpenStreetMap for any shared HTML snapshot.
