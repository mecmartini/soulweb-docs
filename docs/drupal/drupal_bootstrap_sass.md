# Bootstrap (SASS) sub-theme

To generate a new `Bootstrap` (`SASS`) sub-theme we can use:

* [Drupal 8 Bootstrap Subtheme generator](https://github.com/mecmartini/drupal8-bootstrap-subtheme-gen)

The script creates the `Bootstrap` sub-theme and build the `grunt` dependencies and config to process the theme files.

You can choose to run it on your local machine or, if you are using `Drupal VM`, on your `vagrant` machine.

## Requirements

As described on the repository instructions you need:

* `Node.js`
* `Npm`
* `Grunt`

#### Local machine

To install `Node.js` and `Npm` follow the instruction at [Install Node.js using Nvm](drupal_vm_eslint.md#1-install-nodejs-using-nvm).

Then run:

    sudo npm install -g grunt-cli

#### Vagrant machine

Open the vagrant machine `config.yml` file and make sure to have the `nodejs` line uncommented on `installed_extras`

    installed_extras:
          ...
          - nodejs
          ...

Add `grunt` to `Npm` global packages:

    nodejs_npm_global_packages:
      ...
      - grunt-cli

From your `terminal` go on the `vagrant` directory and run `vagrant up --provision`, to apply the changes on your `vagrant` machine, or run `vagrant provision` if your machine is already up.

## Usage

Follow the instructions of `Composer generator` on the link at the top.

When the script is done you must delete the files:

* `bootstrap_subtheme_composer_gen.sh`
* `get-tag.awk`

To process the theme files move on the created sub-theme directory and run:

* `grunt` to process your file
* `grunt watch` to watch your project files for changes