# Command Line Interface

- [Setup & Maintenance](#maintenance-commands)
    - [System Update](#console-update-command)
    - [Database Migration](#console-up-command)
    - [Change Backend User Password](#change-backend-user-password-command)
- [Project Management](#project-commands)
    - [Synchronize Project](#project-sync-command)
    - [Set Project](#project-set-command)
- [Plugin Management](#plugin-commands)
    - [Install Plugin](#plugin-install-command)
    - [Check Dependencies](#plugin-check-command)
    - [Refresh Plugin](#plugin-refresh-command)
    - [List Plugin](#plugin-list-command)
    - [Disable Plugin](#plugin-disable-command)
    - [Enable Plugin](#plugin-enable-command)
    - [Remove Plugin](#plugin-remove-command)
- [Theme Management](#theme-commands)
    - [Install Theme](#theme-install-command)
    - [Check Protected](#theme-check-command)
    - [List Themes](#theme-list-command)
    - [Enable Theme](#theme-use-command)
    - [Remove Theme](#theme-remove-command)
    - [Copy Theme](#theme-copy-command)
- [Utilities](#utility-commands)
    - [Clear Application Cache](#cache-clear-command)
    - [Remove Demo Data](#october-fresh-command)
    - [Mirror Public Directory](#cache-clear-command)
    - [Miscellaneous Commands](#october-util-command)

October CMS includes several command-line interface (CLI) commands and utilities that allow you to manage various aspects of the platform, as well as speed up the development process. The console commands are based on Laravel's [Artisan](http://laravel.com/docs/artisan) tool. You may [develop your own console commands](../console/development) or speed up development with the provided [scaffolding commands](../console/scaffolding).

<a name="maintenance-commands"></a>
## Setup & Maintenance

<a name="console-update-command"></a>
### System Update

The `october:update` command will request updates from the October gateway. It will update the core application and plugin files, then perform a database migration.

    php artisan october:update

<a name="console-up-command"></a>
### Database Migration

The `october:migrate` command will perform a database migration, creating database tables and executing seed scripts, provided by the system and [plugin version history](../plugin/updates). The migration command can be run multiple times, it will only execute a migration or seed script once, which means only new changes are applied.

    php artisan october:migrate

The `--rollback` option will reverse all migrations, dropping database tables and deleting data. Care should be taken when using this command. The [plugin refresh command](#plugin-refresh-command) is a useful alternative for debugging a single plugin.

    php artisan october:migrate --rollback

<a name="change-backend-user-password-command"></a>
### Change Backend User Password

The `october:passwd` command allows the password of a backend administrator to be changed via the command line. This is useful if you are locked out of your October CMS install, or for changing the password for the default administrator account.

    php artisan october:passwd username password

For the first argument you may pass either the login name or email address. For the second argument you may optionally pass the desired password, otherwise you will be prompted to enter one.

<a name="project-commands"></a>
## Project Management

October CMS includes these commands for managing your project.

<a name="project-sync-command"></a>
### Synchronize Project

`project:sync` installs all plugins and themes belonging to a project.

    php artisan project:sync

<a name="project-set-command"></a>
### Set Project

`project:set` sets the license key for the current installation.

    php artisan project:set <license key>

<a name="plugin-commands"></a>
## Plugin Management

October CMS includes a number of commands for managing plugins.

<a name="plugin-install-command"></a>
### Install Plugin

`plugin:install` - downloads and installs the plugin by its name. The next example will install a plugin called **AuthorName.PluginName**.

    php artisan plugin:install AuthorName.PluginName

You may install a plugin from a remote source using the `--from` option.

    php artisan plugin:install AuthorName.PluginName --from=git@github.com:authorname/pluginname-plugin.git

Use the `--want` option to specify a target branch or version.

    php artisan plugin:install AuthorName.PluginName --from=git@github.com:authorname/pluginname-plugin.git --want=dev-develop

Use the `--oc` option if your package name has the `oc` prefix.

    php artisan plugin:install AuthorName.PluginName --from=git@github.com:authorname/pluginname-plugin.git --oc

<a name="plugin-check-command"></a>
### Check Dependencies

`plugin:check` - performs a system wide check of installed plugin dependencies. This command will spin over every theme and plugin that is currently installed and check to see if its dependencies are also installed. If it finds any missing requirements, it will attempt to install them.

    php artisan plugin:check

<a name="plugin-refresh-command"></a>
### Refresh Plugin

`plugin:refresh` - destroys the plugin's database tables and recreates them. This command is useful for development.

    php artisan plugin:refresh AuthorName.PluginName

Use the `--rollback` option to only destroy the database tables without recreating them.

    php artisan plugin:refresh AuthorName.PluginName --rollback

You may also specify a version number with the `--rollback` option to stop at a specified version.

    php artisan plugin:refresh AuthorName.PluginName --rollback=1.0.3

<a name="plugin-list-command"></a>
### List Plugins

`plugin:list` - Displays a list of installed plugins and their version numbers.

    php artisan plugin:list

<a name="plugin-disable-command"></a>
### Disable Plugin

`plugin:disable` - Disable an existing plugin.

    php artisan plugin:disable AuthorName.PluginName

<a name="plugin-enable-command"></a>
### Enable Plugin

`plugin:enable` - Enable a disabled plugin.

    php artisan plugin:enable AuthorName.PluginName

<a name="plugin-remove-command"></a>
### Remove Plugin

`plugin:remove` - destroys the plugin's database tables and deletes the plugin files from the filesystem.

    php artisan plugin:remove AuthorName.PluginName

<a name="theme-commands"></a>
## Theme Management

October includes a number of commands for managing themes.

<a name="theme-install-command"></a>
### Install Theme

`theme:install` - download and install a theme from the [Marketplace](https://octobercms.com/themes/). The following example will install the theme in `/themes/authorname-themename`

    php artisan theme:install AuthorName.ThemeName

You may install a theme from a remote source using the `--from` option.

    php artisan theme:install AuthorName.ThemeName --from=git@github.com:authorname/themename-theme.git

Use the `--want` option to specify a target branch or version.

    php artisan theme:install AuthorName.ThemeName --from=git@github.com:authorname/themename-theme.git --want=dev-develop

Use the `--oc` option if your package name has the `oc` prefix.

    php artisan theme:install AuthorName.ThemeName --from=git@github.com:authorname/oc-themename-theme.git --oc

<a name="theme-check-command"></a>
### Check Protected

`theme:check` - performs a system wide check of themes to see if they should be flagged read-only and protected from changes. This command will spin over every theme and check if it has been installed with composer, if so, a [theme lock file](../cms/themes#child-themes-lock) is added and a child theme is created.

    php artisan theme:check

<a name="theme-list-command"></a>
### List Themes

`theme:list` - list installed themes.

    php artisan theme:list

<a name="theme-use-command"></a>
### Enable Theme

`theme:use` - switch the active theme. The following example will switch to the theme in `/themes/rainlab-vanilla`

    php artisan theme:use vanilla

<a name="theme-remove-command"></a>
### Remove Theme

`theme:remove` - delete a theme. The following example will delete the directory `/themes/rainlab-vanilla`

    php artisan theme:remove vanilla

<a name="theme-copy-command"></a>
### Copy Theme

`theme:copy` - duplicates an existing theme to create a new one, including the creation of child themes.

    php artisan theme:copy demo

The above command will create a new theme called `demo-copy` by copying the directory and its contents. The `.themelock` file will be removed during this process.

To specify the new directory name, such as `mydemo`, you may supply this a second argument.

    php artisan theme:copy demo mydemo

To create a child theme that inherits the parent theme, specify the `--child` option.

    php artisan theme:copy demo mychild --child

<a name="utility-commands"></a>
## Utilities

October CMS includes a number of utility commands.

<a name="cache-clear-command"></a>
### Clear Application Cache

`cache:clear` - clears the application, twig and combiner cache directories. Example:

    php artisan cache:clear

<a name="october-fresh-command"></a>
### Remove Demo Data

`october:fresh` - removes the demo theme and plugin that ships with October CMS.

    php artisan october:fresh

<a name="cache-clear-command"></a>
### Mirror Public Directory

`october:mirror` - will mirror all asset and resource files to the [public folder](../setup/configuration#public-folder) using symbolic linking.

    php artisan october:mirror

If you want to specify a custom folder, you can pass it as the second argument, which is relative to the base directory.

    php artisan october:mirror mypublicfolder

<a name="october-util-command"></a>
### Miscellaneous Commands

`october:util` - a generic command to perform general utility tasks, such as cleaning up files or combining files. The arguments passed to this command will determine the task used.

#### Compile Assets

Outputs combined system files for JavaScript (js), StyleSheets (less), client side language (lang), or everything (assets).

    php artisan october:util compile assets
    php artisan october:util compile lang
    php artisan october:util compile js
    php artisan october:util compile less

To combine without minification, pass the `--debug` option.

    php artisan october:util compile js --debug

#### Pull All Repos

This will execute the command `git pull` on all theme and plugin directories.

    php artisan october:util git pull

#### Purge Thumbnails

Deletes all generated thumbnails in the uploads directory.

    php artisan october:util purge thumbs

#### Purge Uploads

Deletes files in the uploads directory that do not exist in the `system_files` table.

    php artisan october:util purge uploads

#### Purge Orphans

Deletes records in `system_files` table that do not belong to any other model.

    php artisan october:util purge orphans
