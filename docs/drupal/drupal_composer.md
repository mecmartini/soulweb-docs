# Using Composer

## Add Contrib Modules

Default method to add `Drupal` module through is:

    composer require drupal/<modulename>

You can specify a version from the command line with:

    composer require drupal/<modulename>:<version>

!!! note "Tip"
    To avoid problems on different terminals/shells, surround the version using double quotes. Also, to make sure you will require versions of your dependencies that will guarantee not to break other things, try to use the best combination to constrain versions. Check an example:

        composer require "drupal/ctools:^3.0@alpha"

If you wish to select the `module` version:

    composer require drupal/<modulename> --prefer-dist

## Add other dependencies

    composer require <vendor>/<modulename>

## Managing dependencies for a custom project (module, theme, profile, etc.)

[Managing dependencies for a custom project](https://www.drupal.org/node/2822349)

You can use Composer to manage dependencies for your custom modules. To do this, your Drupal site's composer.json (located in the repo root) must have a way to read your custom project's composer.json file. Since your custom project is not hosted on Packagist or Drupal.org, you must use the wikimedia/composer-merge-plugin` package to accomplish this.

#### 1. Merging in additional composer.json files

Require the `wikimedia/composer-merge-pluginin` your Drupal site's `composer.json` (located in the repo root).

    composer require wikimedia/composer-merge-plugin

Reference your additional composer.json files in the extra section of your root composer.json file.

    "extra": {
      "merge-plugin": {
        "require": [
          "docroot/modules/custom/example/composer.json"
        ]
      }
    }

Update your Drupal site dependencies:

    composer update

#### 2. Include project dependencies in root composer.json

If you wish to avoid using merge-plugin in the process of developing a Drupal module that needs composer package dependencies, you can follow this process to get the packages working in your local development environment.

Simply take the dependencies from your module in development, and add them to the root composer.json file:

    composer require <vendor>/<modulename>

After you save the composer.json file, run `composer update from the same directory as the composer.json file.

Your dependencies will be added to the root /vendor directory, and will be detected by the autoloader as expected.

Be sure to also run `drush cr` or otherwise clear caches for best results.

## Update Drupal core

    composer update drupal/core --with-dependencies

## Update all Drupal dependencies

    composer update

## Define the directories to which Drupal projects should be downloaded

By default, Composer will download all packages to the "vendor" directory. Clearly, this doesn't jibe with Drupal modules, themes, profiles, and libraries. To ensure that packages are downloaded to the correct path, Drupal uses the composer/installers package and ships with configuration for the directories for your Drupal site. The drupal/drupal template does not ship with drupal-libary configuration, but you can just add it to your composer.json:

    "extra": {
        "installer-paths": {
            "core": ["type:drupal-core"],
            "libraries/{$name}": ["type:drupal-library"],
            "modules/contrib/{$name}": ["type:drupal-module"],
            "profiles/contrib/{$name}": ["type:drupal-profile"],
            "themes/contrib/{$name}": ["type:drupal-theme"],
            "drush/{$name}": ["type:drupal-drush"],
            "modules/custom/{$name}": ["type:drupal-custom-module"],
            "themes/custom/{$name}": ["type:drupal-custom-theme"]
        }
    }

## Define the directories for arbitrary packages that do not have a "drupal-*" type

If you would like to place an arbitrary Composer package in a custom directory, you may use the Composer Installers Extender.

For instance, if you'd like to place the Dropzone package (which does not have a type of drupal-library) in the same directory as other Drupal libraries, you would first composer require oomphinc/composer-installers-extender, then add the following configuration to your composer.json file:

    "extra": {
        "installer-paths": {
            "libraries/{$name}": [
                "type:drupal-library",
                "enyo/dropzone"
            ],
        }
    }

Finally, you would composer require enyo/dropzone.

## Apply patches to downloaded modules

If you need to apply patches (depending on the project being modified, a pull request is often a better solution), you can do so with the composer-patches plugin.

To add a patch to drupal module foobar insert the patches section in the extra section of composer.json:

    "extra": {
        "patches": {
            "drupal/foobar": {
                "Patch description": "URL to patch"
            }
        }
    }