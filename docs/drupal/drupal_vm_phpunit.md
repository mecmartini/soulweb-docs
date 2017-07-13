# PHPUnit

#### 1. Integrate PHPUnit in PhpStorm

Copy the `web/core/phpunit.xml.dist` file to `web/core/phpunit.xml` under your `drupal` installation directory.

Edit the created file with your favorite editor and full fill the parameters `SIMPLETEST_DB`, `SIMPLETEST_BASE_URL` and `BROWSERTEST_OUTPUT_DIRECTORY` (see the eamples provided into the file).

Go under `Settings` > `Language & Frameworks` > `PHP` > `PHPUnit`. Click on the `+` button and select `By Remote Interpreter`

![PhpStorm PHPUnit settings](../img/drupal/phpstorm_34.png "PhpStorm PHPUnit settings")

Set the `Interpreter` as below. In `Path to script` and `Default configuration file` substitute the directory path of your `vagrant` machine

![PHPUnit By Remote Interpreter](../img/drupal/phpstorm_50.png "PHPUnit By Remote Interpreter")

#### 2. Set and Run PHPUnit Test

To run all `drupal` tests go under `Edit configurations`. Click the `+` button and select `PHPUnit`

![PHPUnit Edit configurations](../img/drupal/phpstorm_35.png "PHPUnit Edit configurations")

![PHPUnit Add New Configuration](../img/drupal/phpstorm_36.png "PHPUnit Add New Configuration")

Set only the `Name` and the `Test Scope` as below:

![PHPUnit Configuration](../img/drupal/phpstorm_37.png "PHPUnit Configuration")

To test if it works select your `PHPUnit` config and click on `run` (`play button`)

![PHPUnit Run](../img/drupal/phpstorm_38.png "PHPUnit Run")

![PHPUnit in execution](../img/drupal/phpstorm_39.png "PHPUnit in execution")

You can create as many `PHPUnit` configuration do you need, to run subset of test, setting the `Test Runner options`.

The example below shows how to setup it to run only the test of the group `devel`:

![PHPUnit run group devel test](../img/drupal/phpstorm_51.png "PHPUnit run group devel test")

The `--group devel` options is added to the `Test Runner options`.

To run multiple groups of tests:

    --group Group1,Group2,..

To exclude tests:

    --exclude-group Groupname

To run a specific method:

    --filter=MyMethodTest