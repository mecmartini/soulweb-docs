# Configuration Management

## Introduction to Drupal CMI

[Configuration Management](https://www.drupal.org/docs/8/configuration-management)

First of all, you need to understand, how the configuration management in Drupal 8 works. CMI allows you to export all configurations and its dependencies from the database into yml text files. To make sure, you never end up in an inconsistent state, CMI always exports everything. By default, you cannot exclude certain configurations.

To export the configuration you must set the export directory in your Drupal settings. See [Configuration directory](drupal_basic_configuration.md#5-configuration-directory).

Than you can export with:

    drush config-export

If you change some configuration on the database, these configurations will be reverted in the next deployment when you use:

    drush config-import

This is helpful and will make sure, you have the same configuration on all your systems.

## Different configurations on Dev / Stage / Prod environments

You want to have different configurations on your environments. For example, we have installed a “devel” module only on our local environment but we want to have it disabled on the live environment.

This can be achieved by using the [Configuration Split](https://www.drupal.org/project/config_split) module.

Configuration split exposes a configuration entity which controls what you want to split off. Currently you can

* `blacklist modules`: any configuration that this module owns will automatically be blacklisted too.
* `blacklist configuration`: settings or configuration entities. These will be removed from the active sync directory.
* `graylist configuration`: settings or configuration entities. These will not be removed if they are in the active sync directory, but also not exported if they are not there yet.

As default, we have `Dev` / `Stage` / `Prod` config environments already set up and configured on our drupal projects. Those are set to inactive. You must enable the `Dev` environment on your local environment to import / export is config. Open the `settings.local.php` and add this line:

    $config['config_split.config_split.dev']['status'] = TRUE;

If those config environments are not yet set, you can create them when needed but be sure to name them with the following `machine name`:

* `dev`
* `stage`
* `prod`

as those are the defaults for our projects.

Now you can use normal `drush` import /export commands and `Configuration Split` we'll do the magic for you:

    # To export configuration
    drush cex -y
    # To import configuration
    drush cim -y



