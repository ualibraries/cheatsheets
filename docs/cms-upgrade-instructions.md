# Drupal Site Updates

## Important notes about specific sites
Check the [Web Application Scanning and Security wiki page](http://redmine.library.arizona.edu/projects/webapp-scanning/wiki/Important_Notes_about_upgrading_Drupal_7_sites) (University of Arizona Staff Access Only) in Redmine for important notes about the specific Drupal instances. This includes specific information about speccoll.library.arizona.edu, uair.library.arizona.edu, and data.library.arizona.edu. **It will be very important to read these notes before installing updates**. 

## Using Phing (Mainsite)
0. Read the release notes
1. Check you are checkedout on master and pull the latest changes
2. Checkout a feature branch for the update
3. Edit `mainsite.make` and update the version of the plugin you want to upgrade or update the version of Drupal core if you're updating to a new version of Drupal.
4. Run `phing deply_to_dev` and test it on your local build
5. Once your comfortable with your changes to `mainsite.make`, commit them to  your branch
6. If this is a core upgrade, requires any database updates, or if it's a plugin  that you suspect could break on update, consider deploying on test.library.arizona.edu for testing before deploying to production.
7. Merge your branch into develop, then merger it into master. Tag master and push develop, master and the new tag.
8. Deploy to production by running `phing deploy_to_prod`

## Using Drush
0. Read the release notes
1. cd into the Drupal project root
2. Make a new directory in your home directory, named using the following format  
```mkdir ~/drupalprojectnameYYYYMMDD```
3. (Drupal Core Update) Copy `robots.txt`, `.htaccess`, and `settings.php` into you the backup directory you just made  
```cp .htaccess robots.txt sites/default/settings.php ~/drupalprojectnameYYYYMMDD/```
4. Get the credentials for the database from settings.php  
```head --lines=250 sites/default/settings.php```
5. Make a copy of the database by running mysqldump and place the resulting file in the backup directory in your home directory  
```mysqldump -u username -p databasename > ~/drupalprojectnameYYYYMMDD/db-backup.sql```
6. Put the site into maintenance mode  
```drush vset --exact maintenance_mode 1```  
```drush cache-clear all``
7. Run `drush up` to get the names of the modules you want to update (make sure when you are asked if you want to proceed you select no, otherwise all updates will be applied)  
```drush up```   
```Do you really want to continue with the update process? (y/n): n```
8. Use the name (the name you use will be in parentheses next to the full name) to run the update on that specific module or drupal core  
```drush up modulename```
9. Assuming no errors come up, check in the admin interface that the module or drupal core is indeed udated by going to the module list and checking the version number
10. (Drupal Core Update) Restore `robots.txt` and  `.htaccess` (update and restore `settings.php` if the release notes indicate that changes have been made to `default.settings.php`)  
``` cp ~/drupalprojectnameYYYYMMDD/robots.txt ~/drupalprojectnameYYYYMMDD/.htaccess .```
11. If everything went well, take the site out of maintenance mode  
```drush vset --exact maintenance_mode 0```  
```drush cache-clear all``