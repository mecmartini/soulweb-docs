# CORS Configuration

`Cross-origin resource sharing` (`CORS`) is a mechanism that allows a web page to make XMLHttpRequests to another domain. Such `cross-domain` requests would otherwise be forbidden by web browsers, per the same origin security policy.

To enable `CORS in your Drupal installation three methods are available:

* [CORS Module](drupal_cors.md#cors-module)
* [CORS services.yml](drupal_cors.md#cors-servicesyml)
* [CORS settings.environment.php](drupal_cors.md#cors-settingenvironmentphp)

## CORS Module

[CORS](https://www.drupal.org/project/cors) module provides a configuration page to map domains to paths and add the necessary Access-Control-Allow-Origin header.

## CORS services.yml
`
Drupal `services.yml` (located in `sites/default) contain the drupal default method to enable and configure CORS.

Below a configuration example:

       # Configure Cross-Site HTTP requests (CORS).
       # Read https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS
       # for more information about the topic in general.
       # Note: By default the configuration is disabled.
      cors.config:
        enabled: true
        # Specify allowed headers, like 'x-allowed-header'.
        allowedHeaders: ['Content-Type,X-Auth-Token,X-Requested-With,Origin,Authorization,Accept,X-CSRF-Token']
        # Specify allowed request methods, specify ['*'] to allow all possible ones.
        allowedMethods: ['POST,GET,PUT,DELETE,OPTIONS']
        # Configure requests allowed from specific origins.
        allowedOrigins: ['http://example.dev']
        # Sets the Access-Control-Expose-Headers header.
        exposedHeaders: false
        # Sets the Access-Control-Max-Age header.
        maxAge: false
        # Sets the Access-Control-Allow-Credentials header.
        supportsCredentials: true

## CORS settings.environment.php

For complex `CORS` configuration you can use one of `settings.environment.php` in your Drupal installation (e.g. `settings.shared.php`, `settings.local.php`, `settings.dev.php`, etc.) to add your `CORS` configuration.

Below a configuration example:

    header("Access-Control-Allow-Origin: http://example.dev");
    header("Access-Control-Allow-Credentials: true");
    header("Access-Control-Allow-Methods: POST,GET,PUT,DELETE,OPTIONS");
    header("Access-Control-Allow-Headers: Content-Type,X-Auth-Token,X-Requested-With,Origin,Authorization,Accept,X-CSRF-Token");

## CORS multi-origin

For multi-origin CORS configuration the best way is to configure it through [CORS settings.environment.php](drupal_cors.md#cors-settingenvironmentphp).

Below a configuration example:

    $allowed_origin = array(
      'http://example.one.dev',
      'http://example.two.dev',
    );

    if (in_array($_SERVER['HTTP_ORIGIN'], $allowed_origin)) {
      header("Access-Control-Allow-Origin: " . $_SERVER['HTTP_ORIGIN']);
      header("Access-Control-Allow-Credentials: true");
      header("Access-Control-Allow-Methods: POST,GET,PUT,DELETE,OPTIONS");
      header("Access-Control-Allow-Headers: Content-Type,X-Auth-Token,X-Requested-With,Origin,Authorization,Accept,X-CSRF-Token");
    }