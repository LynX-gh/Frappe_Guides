# Guide-to-Backup-and-Restore-Frappe-Site (with Apps and Data)

### STEP 1: Backup

Create a backup of your site with all the files.

Backing up with the site's private and public files.

    bench --site {sitename} backup --with-files

This command wil create 3 backup files. Access these files created after backing up your site at the following location.

    /frappe-bench/sites/{YourSiteName}/private/backups

**Copy these files into your new frappe-bench.**

### STEP 2: Create a repository of your existing custom Apps

    Create a new repository for each custom app of your current site.

### STEP 3: New-Site Setup

- Create a new site.
- Get default apps.
- Get Custom App from the repository

Run the following commands:

    bench get-app {repository link}

- Install apps on the new site.

### STEP 4: Restore

Restoring the files (Database, public, private).

    bench --site {site} restore {path/to/database/file}
     --with-public-files {path/to/public/archive}
     --with-private-files {path/to/private/archive}

Start your new frappe bench.
