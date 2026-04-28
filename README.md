# Odoo 19 Devbox

## Usage

### 1. Start the shell (from the project root):

Use direnv

```bash
cd ~/odoo/dev-odoo/dev-odoo-19
devbox shell
```

This downloads all Nix packages on first run (Python, PostgreSQL, wkhtmltopdf, etc.) and drops you into an isolated shell with everything on PATH.

### 2. Run the setup (first time only, inside the devbox shell):

```bash
devbox run setup
```

This creates the Python venv, installs Odoo + pip deps, and initializes PostgreSQL with the odoo user.

### 3. Start services

Every time you open a new terminal for this project, run `devbox shell` first (or use direnv to auto-activate it).

**Recommended:**
You can still start services individually if you prefer:

```bash
devbox services start   # starts PostgreSQL in background
devbox run start-odoo   # starts Odoo on port 50019
```

Or just use F5 in VS Code (the launch config points to .venv/bin/odoo).

**Use process-compose for unified logs and monitoring:**
This launches all services (PostgreSQL, etc.) together using process-compose, with unified logs and monitoring. Press <Ctrl+C> to stop all.

```bash
devbox services up
```

### 4. Update Odoo dependencies after Git pulling changes

```bash
cd odoo
git pull
devbox run setup
```

## odoo.conf

#### addons_path

Example.\
Whereas `enterprise` is a symlink.

`addons_path = enterprise,addons`

#### http_port

Example:\
`50019`

## PostgreSQL

`psql -h db -U odoo postgres`

## Technologies and tools

**Devbox**

- GitHub: https://github.com/jetify-com/devbox
- Docs: https://www.jetify.com/docs/devbox
- Official website: https://www.jetify.com/devbox

## Monitoring services with process-compose

To list the process-compose processes:
```bash
devbox services ls
```

To attach the process-compose processes:
```bash
devbox services attach
```

## Troubleshooting: PostgreSQL data directory errors

### If you see startup errors, try:

1. Stopping all devbox services:
    ```bash
    devbox services stop
    ```
2. Removing any leftover .devbox/virtenv/postgresql/data/postmaster.pid file if it exists

3 .Starting services again:
    ```bash
    devbox services up
    ```

### If you see errors like:

	 postgres: could not access the server configuration file .../postgresql.conf
	 postgres: could not access directory .../data: No such file or directory

This means the PostgreSQL data directory is missing or empty. To fix:

1. Stop all services:
	```bash
	devbox services stop
	```
2. Remove the broken data directory:
	```bash
	rm -rf .devbox/virtenv/postgresql/data
	```
3. Re-run setup or start services:
	```bash
	devbox run setup
	# or
	devbox services up
	```
This will re-initialize the database and fix the error.
