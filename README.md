# Dockerized Flarum project

Src:
https://github.com/crazy-max/docker-flarum/tree/master/examples/compose


## Push to server:
`rsync -avz --progress /path/to/local/flarum-directory/ user@your-server.com:/path/to/remote/directory/`

### Flag explanations:

- `-a` = Archive mode (preserves permissions, timestamps, etc.)
- `-v` = Verbose (shows what's being transferred)
- `-z` = Compress during transfer
- `--progress` = Shows transfer progress

### Important notes:

- The trailing / on source directory matters! ~/flarum/ syncs contents, ~/flarum syncs the directory itself
- Make sure the destination directory exists or rsync can create it
- First run might take a while, subsequent runs are much faster (only transfers changes)

- Dry run first: 
This shows what would be transferred without actually doing it.

`bashrsync -avz --progress --dry-run ~/flarum/ user@your-server.com:/opt/flarum/`

## env var items to scramble:

Database Credentials (High Priority)
- MYSQL_PASSWORD - Critical - Database password
- DB_PASSWORD - Critical - Same as above, used by Flarum to connect
- MYSQL_USER - Important - Database username (could use something other than default "flarum")
- DB_USER - Important - Same as above
- MYSQL_DATABASE / DB_NAME - Moderate - Database name (could use something other than default "flarum")

Application Security
- DB_PREFIX - Moderate - Change from default "flarum_" to something unique like "mysite_" (makes DB structure less predictable)

What you can keep as defaults:
- PUID / PGID - These are just user/group IDs for file permissions
- TZ - Your timezone
- MEMORY_LIMIT, UPLOAD_MAX_SIZE - Performance settings
- FLARUM_BASE_URL - This needs to be your actual domain
- FLARUM_FORUM_TITLE - Your actual forum name
- DB_HOST - Should stay as "db" (the service name)
- DB_PORT - Standard MySQL port 3306