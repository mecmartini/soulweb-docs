# Development Envirorment

## Drupal VM

[Drupal VM](https://www.drupalvm.com/) is A VM for local Drupal development, built with Vagrant + Ansible.

* [Quick Start Guide](https://github.com/geerlingguy/drupal-vm#quick-start-guide)
* [Drupal VM Documentation](http://docs.drupalvm.com/)  

## Requirements

#### 1. Virtualbox and Vagrant
Download and install [Vagrant](Vagrant) and [VirtualBox](https://www.virtualbox.org/wiki/Downloads) and keep them updated.

#### 2. Xcode
Install [Xcode](https://developer.apple.com/xcode/).

#### 3. Ansible
Install [Ansible](https://valdhaus.co/writings/ansible-mac-osx/) via `pip`.

Open a `Terminal` and run:

    sudo easy_install pip
    sudo pip install ansible --quiet  
      
To update `Ansible`:

    sudo pip install ansible --upgrade

#### 4. Vagrant Plugins
Install the needed Vagrant plugins.

From the `Terminal` run:

    vagrant plugin install vagrant-vbguest
    vagrant plugin install vagrant-hostsupdater
    vagrant plugin install vagrant-auto_network
    vagrant plugin install vagrant-cachier

## Build Drupal VM from scratch

#### 1. Download
Clone the `Drupal VM` project. From the `Terminal` run:

    git clone https://github.com/geerlingguy/drupal-vm.git yourprojectnamevm

Enter on the created folder `yourprojectnamevm`.
   
#### 2. config.yml
The main configuration file of the project. Commonly this is a copy of `default.config.yml` with the values tweaked to your own project.

Copy `default.config.yml` as `config.yml`.

Open the `config.yml` with your favorite editor and edit the following lines:

    vagrant_hostname: yourprojectnamevm.dev
    vagrant_machine_name: yourprojectnamevm
    vagrant_ip: 0.0.0.0

Set the `local` and `remote` (*vagrant*) folders to sync:

    vagrant_synced_folders:
      # The first synced folder will be used for the default Drupal installation, if
      # any of the build_* settings are 'true'. By default the folder is set to
      # the drupal-vm folder.
      - local_path: ~/Sites/yourprojectnamevm
        destination: /var/www
        type: nfs
        create: true

Configure the `drupal composer install dir` to the directory destination of above:

    drupal_composer_install_dir: "/var/www/yourprojectnamevm/drupal”

By default, the `Drupal VM` includes extras packages listed under `installed_extras`. If you don't want or need one or more of those extras, just comment out/in them from the list. Our default list is:

    installed_extras:
      - adminer
      # - blackfire
      - drupalconsole
      - drush
      # - elasticsearch
      # - java
      - mailhog
      # - memcached
      # - newrelic
      # - nodejs
      - pimpmylog
      # - redis
      # - ruby
      # - selenium
      # - solr
      # - tideways
      # - upload-progress
      # - varnish
      - xdebug
      # - xhprof

Select the desidered php version. Currently-supported versions: `5.6`, `7.0`, `7.1`. Our default for  `Drupal 8` projects is `7.1`:

    php_version: “7.1"

Set `php memory limit` at least to `256M`:

    php_memory_limit: "256M"
    php_opcache_memory_consumption: "256"

Continue to modify config.yml to your liking.

#### 3. Build up
Open Terminal, `cd` to the vagrant directory (containing the Vagrantfile and the config.yml file).

Type in `vagrant up`, and let `Vagrant` do its magic.

When it’s done, open the browser and type your `vagrant_hostname` (e.g. `drupaltest.dev`), in the address bar, to navigate on your drupal installation.

Default Drupal credentials to login are specified in your `config.yml`

    drupal_account_name: admin
    drupal_account_pass: admin

At the address `dashboard.your_vagrant_hostname.dev` (e.g. `dashboard.drupaltest.dev`) you can see your `DrupalVM` dashboard.

## Build Drupal VM from existing Drupal project

This is in the scenario where you have an existing existing drupal project (`composer` based) on `git` and you want to start a new `Drupal VM` to local development.

#### 1. Download
Clone the `Drupal VM` project. From the `Terminal` run:

    git clone https://github.com/geerlingguy/drupal-vm.git yourprojectnamevm

Enter on the created folder `yourprojectnamevm`.

#### 2. config.yml
The main configuration file of the project. Commonly this is a copy of `default.config.yml` with the values tweaked to your own project.

Copy `default.config.yml` as `config.yml`.

Open the `config.yml` with your favorite editor and edit the following lines:

    vagrant_hostname: yourprojectnamevm.dev
    vagrant_machine_name: yourprojectnamevm
    vagrant_ip: 0.0.0.0

Set the `local` and `remote` (*vagrant*) folders to sync:

    vagrant_synced_folders:
      # The first synced folder will be used for the default Drupal installation, if
      # any of the build_* settings are 'true'. By default the folder is set to
      # the drupal-vm folder.
      - local_path: ~/Sites/yourprojectnamevm
        destination: /var/www
        type: nfs
        create: true

Configure the `drupal composer install dir` to the directory destination of above:

    drupal_composer_install_dir: "/var/www/yourprojectnamevm/drupal”

Set `Drupal VM` to use your `composer.json` in order to automatically install your `composer` dependencies:

    drupal_build_makefile: false
    ...
    drupal_composer_path: false
    ...
    drupal_build_composer: false
    ...
    drupal_build_composer_project: false

We need to disable the automatic drupal install site because, for now, in this scenario it doesn't work:

    drupal_install_site: false

We'll manually install it later (see [Manually install Drupal site](drupal_vm.md#5-manually-install-drupal-site))

By default, the `Drupal VM` includes extras packages listed under `installed_extras`. If you don't want or need one or more of those extras, just comment out/in them from the list. Our default list is:

    installed_extras:
      - adminer
      # - blackfire
      - drupalconsole
      - drush
      # - elasticsearch
      # - java
      - mailhog
      # - memcached
      # - newrelic
      # - nodejs
      - pimpmylog
      # - redis
      # - ruby
      # - selenium
      # - solr
      # - tideways
      # - upload-progress
      # - varnish
      - xdebug
      # - xhprof

Select the desidered php version. Currently-supported versions: `5.6`, `7.0`, `7.1`. Our default for  `Drupal 8` projects is `7.1`:

    php_version: “7.1"

Set `php memory limit` at least to `256M`:

    php_memory_limit: "256M"
    php_opcache_memory_consumption: "256"

Continue to modify config.yml to your liking.

#### 3. Download your Drupal project site

Clone your `Drupal` project site in the folder set as `local_path` before on your `config.yml` (e.g. `~/Sites/yourprojectnamevm`):

    git clone https://github.com/yourrepository/projectname.git yourprojectnamevm

We'll use the [Configuration Installer](https://www.drupal.org/project/config_installer) profile to install your drupal site with your configuration.

Be sure to already have it in your `Drupal` project as `composer` dependencies (check the `composer.json`). If not, add it to your `config.yml`:

    drupal_composer_dependencies:
      ...
      - "drupal/config_installer"

#### 4. Build up
Open Terminal, `cd` to the vagrant directory (containing the Vagrantfile and the config.yml file).

Type in `vagrant up`, and let `Vagrant` do its magic.

#### 5. Manually install Drupal site

When the `VM` is up and running, enter on it (`vagrant ssh`) and go to your drupal site folder:

    cd /var/www/yourprojectnamevm/drupal/web

Run the `drupal` installation (replace the `db` parameters):

    drush site-install config_installer config_installer_sync_configure_form.sync_directory=../config/sync --db-url=mysql://dbuser:dbpass@127.0.0.1:dbport/dbname --account-name=admin --account-pass=admin -y

where `config_installer_sync_configure_form.sync_directory` is set to the folder that contains your `drupal` default configuration. Our projects `default` is `../config/sync`.

When it’s done, open the browser and type your `vagrant_hostname` (e.g. `drupaltest.dev`), in the address bar, to navigate on your drupal installation.

!!! note "Configuration Split"
    in case your `drupal` project config is split in different folder than the `default`, with [Configuration Split](https://www.drupal.org/project/config_split), and you need to import them too, for each of the split config you need to import run:

        drush csim split_machine_name

    Replace `split_machine_name` with your configuration split `machine name`

Default Drupal credentials to login are specified in your `config.yml`

    drupal_account_name: admin
    drupal_account_pass: admin

At the address `dashboard.your_vagrant_hostname.dev` (e.g. `dashboard.drupaltest.dev`) you can see your `DrupalVM` dashboard.

## Build Drupal VM for multisite

For `multisite` installations, make the changes outlined above, but, using the `apache_vhosts` variable, configure as many domains pointing to the same `docroot` as you need:

    apache_vhosts:
      # Drupal VM's default domain, evaluating to whatever `vagrant_hostname` is set to (drupalvm.dev by default).
      - servername: "{{ drupal_domain }}"
        serveralias: "www.{{ drupal_domain }}"
        documentroot: "{{ drupal_core_path }}"
        extra_parameters: "{{ apache_vhost_php_fpm_parameters }}"

      - servername: "local.second-drupal-site.com"
        documentroot: "{{ drupal_core_path }}"
        extra_parameters: "{{ apache_vhost_php_fpm_parameters }}"

      - servername: "local.third-drupal-site.com"
        documentroot: "{{ drupal_core_path }}"
        extra_parameters: "{{ apache_vhost_php_fpm_parameters }}"

If you need additional databases and database users, add them to the list of `mysql_databases` and `mysql_users`:

    mysql_databases:
      - name: drupal
        encoding: utf8
        collation: utf8_general_ci
      - name: drupal_two
        encoding: utf8
        collation: utf8_general_ci

    mysql_users:
      - name: drupal
        host: "%"
        password: drupal
        priv: "drupal.*:ALL"
      - name: drupal-two
        host: "%"
        password: drupal-two
        priv: "drupal_two.*:ALL"

If you let the `VM` to install your main `drupal` site (`drupal_install_site: true`), be sure to set the appropriate `drupal` host and database for the installation:

    drupal_domain: "{{ vagrant_hostname }}"
    ...
    drupal_db_user: drupal
    drupal_db_password: drupal
    drupal_db_name: drupal

## Updating Drupal VM

Drupal VM follows semantic versioning, which means your configuration should continue working (potentially with very minor modifications) throughout a major release cycle. Here is the process to follow when updating Drupal VM between minor releases:

  1. Read through the [release notes](https://github.com/geerlingguy/drupal-vm/releases) and add/modify `config.yml` variables mentioned therein.
  2. Do a diff of your `config.yml` with the updated `default.config.yml` (e.g. `curl https://raw.githubusercontent.com/geerlingguy/drupal-vm/master/default.config.yml | git diff --no-index config.yml -`).
  3. Run `vagrant provision` to provision the VM, incorporating all the latest changes.

For major version upgrades (e.g. 3.x.x to 4.x.x), it may be simpler to destroy the VM (`vagrant destroy`) then build a fresh new VM (`vagrant up`) using the new version of Drupal VM.