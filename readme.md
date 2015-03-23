# Laravel 4.2 on OpenShift #
[Laravel](http://laravel.com/) is a free, open source PHP web application framework, 
designed for the development of model–view–controller (MVC) web applications.

This QuickStart was created to make it easy to get started with Laravel 4.2 on
OpenShift.

The simplest way to install this application is to use the [OpenShift
QuickStart](https://hub.openshift.com/quickstarts/124-laravel-4-2). If 
you'd like to install it manually, follow [these directions](#manual-installation).

## OpenShift Considerations ##
These are some special considerations you may need to keep in mind when
running your application on OpenShift.

### Local vs. Remote Development ###
The `bootsrap/start.php` has been updated to auto-detect a remote deployment on 
OpenShift and set the environment accordingly. See the following code:

    $env = $app->detectEnvironment(function () {
	    return isset($_ENV['OPENSHIFT_PHP_DIR']) ? 'production' : 'local';
    });

This Laravel QuickStart provides configuration files for both local and 
remote development. The values in the main `app/config` directory will be used by 
default in production on OpenShift. You can override these values in the 
`app/config/local` directory with settings for local development. See 
[environment configuration](http://laravel.com/docs/4.2/configuration#environment-configuration) 
for more information. 

### Remote Development ###
Your application is configured to automatically use your OpenShift MySQL or PostgreSQL 
database in when deployed on OpenShift using [OpenShift Environment Variables](https://developers.openshift.com/en/managing-environment-variables.html).

Additionally, your application URL and encryption key will be set automatically in 
production on OpenShift.

The Laravel cache driver is set to use [APC caching](http://php.net/manual/en/book.apc.php)
and the session driver is set to use the local file system for storage. Feel 
free to update these settings in `app/config/cache.php` and `app/config/session.php`.

### Laravel Migrations ###
When the application is pushed to OpenShift, `php artisan migrate --force` is 
automatically executed.

### Composer ###
When the application is pushed, `composer install` is automatically executed over the 
root directory. See [PHP Markers](https://developers.openshift.com/en/php-markers.html) 
for more details on the 'use_composer' marker.

### 'Development' Mode ###
When you develop your Laravel application in OpenShift, you can also enable the
'development' environment by setting the `APPLICATION_ENV` environment variable,
using the `rhc` client, like:

```
$ rhc env set APPLICATION_ENV=development
```

Then, restart your application:

```
$ rhc app restart -a <app-name>
```

If you do so, OpenShift will run your application under 'development' mode.
In development mode, your application will:

* Enable Laravel's application debug mode
* Ignore your composer.lock file
* Show more detailed errors in browser
* Display startup errors
* Enable the [Xdebug PECL extension](http://xdebug.org/)
* Enable [APC stat check](http://php.net/manual/en/apc.configuration.php#ini.apc.stat)

Set the variable to 'production' and restart your app to deactivate development mode
and resume production PHP settings.

Using the development environment can help you debug problems in your application
in the same way as you do when developing on your local machine. However, we strongly 
advise you not to run your application in this mode in production.

### Log Files ###
Your application is configured to use the OpenShift log directory. You can use the 
`rhc tail` command to stream the latest log file entries:

```
rhc tail -a <APP_NAME>
```

To stop tailing the logs, press *Ctrl + c*.

## Manual Installation ##

1. Create an account at https://www.openshift.com/

1. Create a Laravel application:

    ```
    rhc app create laravelapp https://raw.githubusercontent.com/luciddreamz/openshift-php/master/metadata/manifest.yml mysql-5.5 --from-code=https://github.com/luciddreamz/laravel-4.2
    ```
    or

    ```
    rhc app create laravelapp https://raw.githubusercontent.com/luciddreamz/openshift-php/master/metadata/manifest.yml postgresql-9.2 --from-code=https://github.com/luciddreamz/laravel-4.2
    ```

   **Note:** This QuickStart is setup to use a [custom version](https://github.com/luciddreamz/openshift-php) of the OpenShift PHP 5.4 
   cartridge with the latest version of Composer installed. The version of Composer that 
   ships with OpenShift's current PHP 5.4 cartridge is slightly out of date will error when 
   it encounters Laravel's more recent version syntax (ex. Invalid version string "^1.0.1").

## Additional Resources ##
Documentation for the Laravel framework can be found on the [Laravel website](http://laravel.com/docs). Check 
out OpenShift's [Developer Portal](https://developers.openshift.com/en/php-overview.html) for help running PHP on OpenShift.