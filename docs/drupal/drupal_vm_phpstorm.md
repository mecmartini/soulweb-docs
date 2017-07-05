# PhpStorm Project

Make sure to have the latest [PhpStorm](https://www.jetbrains.com/phpstorm/download) available for a better support.

## Create new project Vagrant based

#### 1. Create new project

Open `PhpStorm` and select `Create New Project from Existing File`

![Create New Project from Existing File](../img/drupal/phpstorm_1.png "Create New Project from Existing File")

Select `Sources file are in a local directory, no Web server is yet configured.`

![Sources file are in a local directory, no Web server is yet configured](../img/drupal/phpstorm_2.png "Sources file are in a local directory, no Web server is yet configured")

Select your `Drupal` installation directory on the local machine, make it the `Project Root` and click on `Finish` button

![Choose Project Directory](../img/drupal/phpstorm_3.png "Choose Project Directory")

Once the project is created, `PhpStorm` will index your project files and must recognise it as a `Drupal` project. It should ask to enable the `Drupal` support. If not, enable it by yourself:

![Enable Drupal support](../img/drupal/phpstorm_4.png "Enable Drupal support")

#### 2. Integrate Vagrant

To let `PhpStorm` find the vagrant executable run the following on your local machine:

    sudo ln -s /usr/local/bin/vagrant /usr/bin/vagrant

!!! note "Note"
    in case the executable is in a different path then `/usr/local/bin` change it, you can find the vagrant path with `whereis vagrant`
    
To integrate `Vagrant` set the `Instance Folder` on the `Vagrant` settings:

![Vagrant settings](../img/drupal/phpstorm_5.png "Vagrant settings")

Then select the `Current Vagrant` on `SSH Terminal` settings:

![SSH Terminal settings](../img/drupal/phpstorm_6.png "SSH Terminal settings")

Create the new Vagrant `Deployment` server clicking on `+` button in the following settings:

![Deployment server](../img/drupal/phpstorm_7.png "Deployment server")

Enter a `Name` and select Type `SFTP`:

![Add server](../img/drupal/phpstorm_8.png "Add server")

Click `OK` button and configure the rest as:

    SFTP host: your vagrant_hostname
    Root path: your Drupal installation path on the vagrant machine
    Username: vagrant 
    Password: vagrant

Go on the `Mappings` tab and set only the `Deployment path on server` with your `Drupal` installation path on the `vagrant machine`

![Deployment path on server](../img/drupal/phpstorm_9.png "Deployment path on server")

Set your vagrant `PHP interpreter`. From the following click on `…` of `CLI Interpreter`:

![CLI Interpreter](../img/drupal/phpstorm_10.png "CLI Interpreter")

Click on `+` button and select `Remote`. Set the interpreter as below, selecting `Vagrant` and setting the `Vagrant Instance Folder` to your `Vagrant` folder:

![CLI Interpreter settings](../img/drupal/phpstorm_11.png "CLI Interpreter settings")

Go to `Settings` -> `PHP` -> `Servers` and click on `+` button:

![Vagrant server settings](../img/drupal/phpstorm_12.png "Vagrant server settings")

Set your `Name` and `Host`. Check `Use path mappings` and enter the `Absolute path on the server` to your `drupal` installation on vagrant machine.

## Add GitHub repository and Initial Commit

Share the project on `GitHub`:

![Share github project](../img/drupal/phpstorm_13.png "Share github project")

Write the `New repository name`, select `private`, if needed, and click on `Share`

![Share github project](../img/drupal/phpstorm_14.png "Share github project")

Add files for `initial commit` and `push` on git by clicking on `OK` button

![Initial commit](../img/drupal/phpstorm_15.png "Initial commit")

## Import to existing GitHub repository and Initial Commit

Enable version control integration from PHPSTORM:

![Enable version control integrations](../img/drupal/phpstorm_16.png "Enable version control integrations")

Set the `Remote` origin of the existing `git` repository adding the repository `url` (e.g. `https://github.com/mecmartini/soulweb-docs.git`)

![Set git remote origin](../img/drupal/phpstorm_17.png "Set git remote origin")

![Set git remote origin](../img/drupal/phpstorm_18.png "Set git remote origin")

![Set git remote origin](../img/drupal/phpstorm_19.png "Set git remote origin")

![Set git remote origin](../img/drupal/phpstorm_20.png "Set git remote origin")

Push the `initial commit` (see [Add GitHub repository and Initial Commit](drupal_vm_phpstorm.md#3-add-github-repository-and-initial-commit)).

## Plugin Requirements

Here are listed the required PhpStorm plugins for our development workflow.

#### Install/Enable PhpStorm Plugin

From your project `settings` go to `Plugin`:

![PhpStorm Plugin Settings](../img/drupal/phpstorm_40.png "PhpStorm Plugin Settings")

Search the required `plugin` on the list and enable it (click on the right checkbox of the `plugin` name).

If it's not listed  you must install it. Press on `Browse Repositories`, search for your `plugin`, select it and click on `Install` on the right side:

![PhpStorm Plugin Browse Repositories](../img/drupal/phpstorm_41.png "PhpStorm Plugin Browse Repositories")

Close the `Browse Repositories` window. Now you'll find it on the list to enable it.

#### Drupal Symfony Bridge Plugin

[Drupal Symfony Bridge Plugin](https://github.com/Haehnchen/idea-php-drupal-symfony2-bridge)

Provides Symfony components support for Drupal in PhpStorm.

#### Pre Commit Hook Plugin

[Pre Commit Hook Plugin](https://github.com/yahely/PreCommitHookPlugin)

Plugin that allows you to run a hook prior commiting changes to any Version Control System. Good for Version Control Systems that doesn't allow you to run pre-commit-hook on the client side.
