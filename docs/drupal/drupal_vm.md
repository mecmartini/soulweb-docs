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

From a `Terminal` run:

    vagrant plugin install vagrant-vbguest
    vagrant plugin install vagrant-hostsupdater
    vagrant plugin install vagrant-auto_network

## Build Drupal VM

#### 1. Download
Clone the Drupal VM project. From a `Terminal` run:

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
        destination: /var/www/yourprojectnamevm
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

Select the desidered php version. Currently-supported versions: 5.6, 7.0, 7.1.:

    php_version: “7.1"

Continue to modify config.yml to your liking.

#### 2. Build up
Open Terminal, `cd` to the vagrant directory (containing the Vagrantfile and the config.yml file).

Type in `vagrant up`, and let `Vagrant` do its magic.

When it’s done, open the browser and type your `vagrant_hostname` (e.g. `drupaltest.dev`), in the address bar, to navigate on your drupal installation.

Default Drupal credentials to login are specified in your `config.yml`

    drupal_account_name: admin
    drupal_account_pass: admin

At the address `dashboard.your_vagrant_hostname.dev` (e.g. `dashboard.drupaltest.dev`) you can see your `DrupalVM` dashboard.

