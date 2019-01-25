# Drupal 8 Bash Function Installer for Valet Plus
A bash function for mac OS with Valet Plus installed

## Requirements:
- Valet Plus
- Composer

## This function will:
 - Download a fresh copy of Drupal 8 to the directory given in parameter 1 using composer
 - Download commonly required development modules `admin_toolbar`, `devel` and `module_filter` using composer
 - Create a database of the name given in parameter 1 using Valet Plus
 - Install Drupal to the database using the site name and database given in parameter 1 (Default password = `password`) using Drush
 - Enable to developemnt modules using Drush
 - Link the web directory to the name given in parameter 1
 - If enabled, will open the project in PhpStorm using valet.
 
 ## Usage:
  - Copy the function to your .bash_profile
  - Open a new terminal to load the new command
  - `Cd` to where the site should be installed. E.G. `/var/www`
  - Run `drupal-install SITENAME`. E.G. `drupal-install drupal-8`


```

#---------- Function List ---------

function drupal-install() {
  composer create-project drupal-composer/drupal-project:8.x-dev $1 --stability dev --no-interaction
  cd $1
  composer require drupal/admin_toolbar drupal/devel drupal/module_filter
  valet db create $1
  drush site:install --site-name $1 --account-pass password --db-url=mysql://root:root@localhost:3306/$1
  drush en admin_toolbar_tools devel kint module_filter -y
  cd web
  valet link $1
  cd ../
  # -- Uncomment the below if you have PHP Storm to automatically open the project --
  # valet phpstorm
}

#----------------------------------
```
