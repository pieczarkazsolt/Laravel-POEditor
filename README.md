# Laravel-POEditor
A Laravel wrapper for the POEditor. In order to use this package you need to have an account and project at [POEditor](https://poeditor.com/). You can retrieve your API token and project id's from your account page in the API Access tab.

* [Installation](#installation)
* [Usage](#usage)
* [Unit Testing](#unit-testing)

## Installation

- [Laravel](#laravel)
- [Lumen](#lumen)

### Laravel

This package can be used in Laravel 5.4 or higher.

You can install the package via composer:

``` bash
composer require sebudesign/laravel-poeditor
```

In Laravel 5.5 the service provider will automatically get registered. In older versions of the framework just add the service provider in `config/app.php` file:

```php
'providers' => [
    // ...
    SeBuDesign\PoEditor\PoEditorServiceProvider::class,
];
```

You can publish the config file with:

```bash
php artisan vendor:publish --provider="SeBuDesign\PoEditor\PoEditorServiceProvider" --tag="config"
```

When published, [the `config/poeditor.php` config file](https://github.com/SeBuDesign/Laravel-POEditor/blob/master/config/poeditor.php) contains:

```php
return [

    /*
     * The API token of POEditor can be found in your account settings in the
     * 'API Access' tab. Click on the little eye ball and copy the token. Put
     * the token in your .env file.
     */

    'api_token' => env('POEDITOR_API_TOKEN'),

    /*
     * The project id of POEditor can be found in your account settings in the
     * 'API Access' tab. Grab the project id and this package will take care of the rest.
     */

    'project_id' => env('POEDITOR_PROJECT_ID', 'YourPoEditorProjectId'),
];
```

Add the POEDITOR_API_TOKEN and POEDITOR_PROJECT_ID to your .env file.

### Lumen

You can install the package via Composer:

``` bash
composer require sebudesign/laravel-poeditor
```

Copy the required files:

```bash
cp vendor/sebudesign/laravel-poeditor/config/poeditor.php config/poeditor.php
```

As well as the configuration and the service provider:

```php
$app->configure('poeditor');
$app->register(SeBuDesign\PoEditor\PoEditorServiceProvider::class);
```

## Usage

- [Automatic](#automatic)
- [Manual](#manual)

### Automatic

Make sure you've updated your .env file with the right API credentials.

If you wish to automatically update your translation files you need to add them to your .gitignore file. The command will create real files and that will cause problems with git and your deployments.
Add the following line to your .gitignore, this will exclude all files generated by the command.
```bash
resources/lang/*.json
```

Next you want to modify your `app/Console/Kernel.php` and add the following line to the schedule function:

```php
protected function schedule(Schedule $schedule)
{
    $schedule->command('synchronise:translations')->hourly();
}
```

### Manual

Make sure you've updated your .env file with the right API credentials.

If you want to update the translations locally and commit them to git and deploy them, you just manually run the `synchronise:translations`. Optionally you may add a project id in the terminal.
```bash
php artisan synchronise:translations
# With POEditor project id, replace 1234 with your project id
php artisan synchronise:translations --project=1234
```

## Unit Testing

In your application's tests, you need to make sure you run the artisan command to synchronise the translations. See [Usage](#usage) for more information.

