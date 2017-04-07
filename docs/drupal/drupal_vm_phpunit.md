# PHPUnit

#### 1. Integrate PHPUnit in PhpStorm

Enable the `Testing` module on your `drupal` installation. Access on your `vagrant` machine (`vagrant ssh`), go under the `drupal` installation directory and run:

    drush en -y testing

Open the file `web/core/phpunit.xml.dist`, under your `drupal` installation directory, and fill in `SIMPLETEST_DB`, `SIMPLETEST_BASE_URL` and `BROWSERTEST_OUTPUT_DIRECTORY`.
Go under `Settings` > `Language & Frameworks` > `PHP` > `PHPUnit`. Click on the `+` button and select `By Remote Interpreter`

![PhpStorm PHPUnit settings](../img/drupal/phpstorm_33.png "PhpStorm PHPUnit settings")

Set the `Interpreter` as below. In `Path to script` and `Default configuration file` enter the directory path of your `vagrant` machine

![PHPUnit By Remote Interpreter](../img/drupal/phpstorm_34.png "PHPUnit By Remote Interpreter")

To run all `drupal` tests go under `Edit configurations`. Click the `+` button and select `PHPUnit`

![PHPUnit Edit configurations](../img/drupal/phpstorm_35.png "PHPUnit Edit configurations")

![PHPUnit Add New Configuration](../img/drupal/phpstorm_36.png "PHPUnit Add New Configuration")

Set only the `Name`

![PHPUnit Configuration](../img/drupal/phpstorm_37.png "PHPUnit Configuration")

To test if it works select your `PHPUnit` config and click on `run` (`play button`)

![PHPUnit Run](../img/drupal/phpstorm_38.png "PHPUnit Run")

![PHPUnit in execution](../img/drupal/phpstorm_39.png "PHPUnit in execution")