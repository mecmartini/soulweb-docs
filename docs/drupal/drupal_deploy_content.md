# Deploy Content

To deploy configuration that depends on content such as nodes, blocks, taxonomy terms, etc.

## Default Content module

[Default Content](https://www.drupal.org/project/default_content) module gives your module and install profile a way to ship default content as well as configuration.

## Deploy module

[Deploy](https://www.drupal.org/project/deploy) module is designed to allow users to easily stage and preview content for a Drupal site. Deploy automatically manages dependencies between entities (like node references). It is designed to have a rich API which can be easily extended to be used in a variety of content staging situations.

## Updating content

In `drupal` 8 there is a post update hook specifically designed to work with content:

    function hook_post_update_NAME(&$sandbox) { }

New with proof of concept module [Config Import N](https://github.com/bircher/drupal-config_import_n).