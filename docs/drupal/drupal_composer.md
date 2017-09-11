# Using Composer

## Add Contrib Modules

Default method to add `Drupal` module through is:

    composer require drupal/<modulename>

You can specify a version from the command line with:

    composer require drupal/<modulename>:<version>

!!! tip "Tip"
    To avoid problems on different terminals/shells, surround the version using double quotes. Also, to make sure you will require versions of your dependencies that will guarantee not to break other things, try to use the best combination to constrain versions. Check an example:

        composer require "drupal/ctools:^3.0@alpha"

If you wish to select the `module` version:

    composer require drupal/<modulename> --prefer-dist

## Add other dependencies

    composer require <vendor>/<modulename>

## Development dependencies

There are often components of your project that you need when doing development work, but you don't need on production. For example, Devel, XHProf, and Stage File Proxy are helpful to have on your local environment, but if you don't need them in production, you should exclude them from your codebase entirely (not only for minor performance reasons and keeping your build artifacts smaller—non-installed modules can still be a security risk if they have vulnerabilities).

Composer lets you track 'dev dependencies' (using require-dev instead of require) that are installed by default, but can be excluded when building the final deployable codebase (by passing --no-dev when running composer install or composer update).

To add these components only for dev you should run:

    composer require --dev <vendor>/<modulename>

In case you are adding a drupal contrib module, remember to exclude it's drupal configuration from the default sync folder through `Configuration Split`

## Managing dependencies for a custom project (module, theme, profile, etc.)

[Managing dependencies for a custom project](https://www.drupal.org/node/2822349)

You can use Composer to manage dependencies for your custom modules.

#### 1. Add composer.json to your project

Add a `composer.json` file to your custom project where you can define your custom library dependencies.

#### 2. Merging in additional composer.json files

To do this, your Drupal site's composer.json (located in the repo root) must have a way to read your custom project's composer.json file. Since your custom project is not hosted on Packagist or Drupal.org, you must use the wikimedia/composer-merge-plugin` package to accomplish this.

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

## Update Drupal core package

    composer update drupal/core --with-dependencies

!!! danger "Danger"
    Leave this to the dependency manager, never run it by yourself!

## Update all packages

    composer update

!!! danger "Danger"
    Leave this to the dependency manager, never run it by yourself!

## Update a package

    composer update [vendor]/[package] --with-dependencies

## Define the directories to which Drupal projects should be downloaded

By default, Composer will download all packages to the `vendor` directory. Clearly, this doesn't jibe with Drupal modules, themes, profiles, and libraries. To ensure that packages are downloaded to the correct path, Drupal uses the composer/installers package and ships with configuration for the directories for your Drupal site. The drupal/drupal template does not ship with drupal-libary configuration, but you can just add it to your composer.json:

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

    composer require composer/installers

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

## Define a 3rd party library as a custom composer package

If you would like to place an arbitrary library, that doesn't have a composer repository, as a custom composer package, you may use the `Composer Installers Extender`:

    composer require composer/installers
    composer require oomphinc/composer-installers-extender

Define your custom repositories:

    "repositories": [
        ...
        {
            "type": "package",
            "package": {
                "name": "enyo/dropzone",
                "version": "4.3",
                "type": "drupal-library",
                "source": {
                    "url": "https://github.com/enyo/dropzone.git",
                    "type": "git",
                    "reference": "master"                },
                "dist": {
                    "url": "https://github.com/enyo/dropzone/archive/v4.3.0.zip",
                    "type": "zip"
                },
                "require": {
                    "composer/installers": "~1.0"
                }
            }
        }
    ]

Make sure to have set the libraries installer path:

    "installer-paths": {
        ...
        "web/libraries/{$name}": [
                    "type:drupal-library"
                ],
        ...
    }

Now you can install your custom package:

    composer require enyo/dropzone

## Manage project assets from NPM or Bower

If you would like to manage your project `assets` (css, js, etc.) in your `composer.json` without installing `NPM` or `Bower`, you may user the `composer` repository [Asset Packagist](https://asset-packagist.org/).

Add the repository to `composer.json`:

    "repositories": [
        ...
        {
            "type": "composer",
            "url": "https://asset-packagist.org"
        }
    ],

This repository works by transposing package information from `NPM` or `Bower` to a compatible version for `Composer`. This allows you to manage asset dependencies in a `PHP` based project very easily.

To install those packages in the `drupal` libraries directory you needed `composer-installers-extender` and `drupal-library-mapping` packages:

    composer require oomphinc/composer-installers-extender bmcclure/drupal-library-mapping

Now you can define the custom directory for the assets installation, according to the standard drupal libraries directory:

    "extra": {
        "installer-types": ["library", "drupal-library", "bower-asset", "npm-asset"],
        "installer-paths": {
            "web/core": ["type:drupal-core"],
            "web/libraries/{$name}": [
                "type:drupal-library",
                "type:bower-asset",
                "type:npm-asset"
            ],
            ...
        }
    }

To avoid the download of dependencies already present in `drupal` core:

    "replace": {
        "bower-asset/jquery": "*",
        "bower-asset/jqueryui": "*",
        "bower-asset/jquery-ui": "*",
        "bower-asset/backbone": "*",
        "bower-asset/underscore": "*",
        "npm-asset/jquery": "*",
        "npm-asset/jqueryui": "*",
        "npm-asset/jquery-ui": "*",
        "npm-asset/backbone": "*",
        "npm-asset/underscore": "*"
    },

Adding a dependency on an asset, you must add the asset to the property require of the `composer.json` of your project.

It must be prefixed with `{asset-type}-asset/`:

    composer require npm-asset/bootstrap
    ...
    composer require bower-asset/bootstrap

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

## Resources

* [Common Problems and Solutions with Drupal + Composer](https://www.jeffgeerling.com/blog/2017/composer-and-drupal-are-still-strange-bedfellows)