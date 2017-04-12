# Basic Configuration

#### 1. .gitignore

Default `.gitignore` to use on our Drupal 8 projects:

    # Ignore directories generated by Composer
    /drush/contrib/
    /vendor/
    /web/core/
    /web/modules/contrib/
    /web/themes/contrib/
    /web/profiles/contrib/
    /web/libraries/

    # Ignore sensitive information
    /web/sites/*/settings.php
    /web/sites/*/settings.local.php

    # Ignore Drupal's file directory
    /web/sites/*/files/

    # Ignore SimpleTest multi-site environment.
    /web/sites/simpletest

    # Ignore files generated by PhpStorm
    /.idea/

You can add more if you need. You must not remove the defaults.

#### 2. Enable local development settings

You have to make the site settings directory (e.g. `default`) and is `settings.php` writable to do this. Drupal will restore permissions in a later moment:

    chmod +w sites/default
    chmod +w sites/default/settings.php

Copy and rename the `sites/example.settings.local.php` to `sites/default/settings.local.php`:

    cp sites/example.settings.local.php sites/default/settings.local.php

Open `settings.php` file in `sites/default` and uncomment these lines:

    if (file_exists(__DIR__ . '/settings.local.php')) {
      include __DIR__ . '/settings.local.php';
    }

This will include the local settings file as part of Drupal's settings file.

#### 3. Disable Drupal caching

Open `settings.local.php` and uncomment (or add) this line to enable the null cache service:

    $settings['container_yamls'][] = DRUPAL_ROOT . '/sites/development.services.yml';

Uncomment these lines in `settings.local.php` to disable the render cache and disable dynamic page cache:

    $settings['cache']['bins']['render'] = 'cache.backend.null';
    $settings['cache']['bins']['dynamic_page_cache'] = 'cache.backend.null';

Open `development.services.yml` in the sites folder and add the following block to disable the `twig` cache:

    parameters:
      twig.config:
        debug: true
        auto_reload: true
        cache: false

Afterwards you have to rebuild the `Drupal` cache otherwise your website will encounter an unexpected error on page reload:

    drush cr

Now you should be able to develop in `Drupal` without manual cache rebuilds on a regular basis.

Your final `development.services.yml` should look as follows (mind the indentation):

    # Local development services.
    #
    # To activate this feature, follow the instructions at the top of the
    # 'example.settings.local.php' file, which sits next to this file.
    parameters:
      http.response.debug_cacheability_headers: true
      twig.config:
        debug: true
        auto_reload: true
        cache: false
    services:
      cache.backend.null:
        class: Drupal\Core\Cache\NullBackendFactory

#### 4. Private files

To use private files on `drupal` you must edit your `settings.php`.

You have to make the site settings directory (e.g. `default`) and is `settings.php` writable to do this. Drupal will restore permissions in a later moment:

    chmod +w sites/default
    chmod +w sites/default/settings.php

Open the site `settings.php`. Uncomment and set the following line with a local file system path where private files will be stored:

    $settings['file_private_path'] = '/var/www/yourprojectnamevm/drupal/private';

This directory must be absolute, outside of the Drupal installation directory and not accessible over the web.

Caches need to be cleared when this value is changed to make the `private://` stream wrapper available to the system.

    drush cr

#### 5. Configuration directory

You have to make the site settings directory (e.g. `default`) and is `settings.php` writable to do this. Drupal will restore permissions in a later moment:

    chmod +w sites/default
    chmod +w sites/default/settings.php

Open `settings.php` and uncomment, edit or add this line to set the configration directory to our default:

    $config_directories['sync'] = '../config/sync';