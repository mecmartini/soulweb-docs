# Multisite

If you are running more than one Drupal site, you can simplify the management and can upgrade your sites by using the multi-site feature. Multi-site allows you to share a single Drupal installation (including core code, contributed modules, and themes) among several sites.

This is particularly useful for managing the code since each upgrade only needs to be done once. Each site will have its own database and its own configuration settings, so each site will have its own content, settings, enabled modules, and enabled theme. However, the sites are sharing a code base and web document root, so there may be security concerns with multiple administrators. (See "[Security Concerns](https://www.drupal.org/docs/7/multisite-drupal/multi-site-sharing-the-same-code-base#security)" for more information).

## From scratch