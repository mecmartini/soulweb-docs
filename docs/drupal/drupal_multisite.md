# Multisite

If you are running more than one Drupal site, you can simplify the management and can upgrade your sites by using the multi-site feature. Multi-site allows you to share a single Drupal installation (including core code, contributed modules, and themes) among several sites.

This is particularly useful for managing the code since each upgrade only needs to be done once. Each site will have its own database and its own configuration settings, so each site will have its own content, settings, enabled modules, and enabled theme. However, the sites are sharing a code base and web document root, so there may be security concerns with multiple administrators. (See "[Security Concerns](https://www.drupal.org/docs/7/multisite-drupal/multi-site-sharing-the-same-code-base#security)" for more information).

## Create a Multisite

You can create a new installation either from scratch or as a sub-site of an existing Drupal installation. The only thing you need is a new empty database.

For a detailed list of steps, you can take a look at [Drupal.org Docs about Multi-site - Sharing the same code base](https://www.drupal.org/docs/7/multisite-drupal/multi-site-sharing-the-same-code-base).

Using drush, you can run this simple command:

    drush site-install --db-url=mysql://db_user:db_password@localhost:port/db_name --sites-subdir=sample.subsite.com --account-name=admin --account-pass=admin -y

To install it with a different language than default (english) you can use the `locale` parameter. As example, to install it with `italian` language you can run:

    drush site-install --db-url=mysql://db_user:db_password@localhost:port/db_name --sites-subdir=sample.subsite.com --account-name=admin --account-pass=admin  --locale=it -y

## Multisite with Shared Configuration

We can use `Configuration Management` (CMI) with [Configuration Split](https://www.drupal.org/project/config_split) to share the configuration between sites (see [Configuration Management](drupal_configuration_management.md)).

## Install a subsite from an existing configuration

With the help of [Configuration Installer](https://www.drupal.org/project/config_installer) we can install a new subsite from an existing configuration.

Add the `Config Installer` profile:

    drush dl config_installer

Install the subsite from the existing configuration:

    drush site-install config_installer config_installer_sync_configure_form.sync_directory=../config/sync --db-url=mysql://dbuser:dbpass@127.0.0.1:dbport/dbname --account-name=admin --account-pass=admin -y

!!! note "Note"
    For more information about `CMI` and `Config Installer` from an existing configuration see:

    * [Build Drupal VM from existing Drupal project](drupal_vm.md#build-drupal-vm-from-existing-drupal-project)
    * [Drupal 8 Configuration Management](drupal_configuration_management.md)

## Configure your multisite domains and aliases with sites.php

On your `drupal` installation, under `sites/default` copy the file `example.sites.php` in `sites` folder and configure your subsite domains and aliases as below:

    $sites = array(
      'example.dev' => 'it.example.dev',
      'example.com' => 'it.example.dev',
      'it.example.dev' => 'it.example.dev',
      'it.example.com' => 'it.example.dev',
      'es.example.dev' => 'es.example.dev',
      'es.example.com' => 'es.example.dev',
    );

You can find more info about how to configure it in the file instead.