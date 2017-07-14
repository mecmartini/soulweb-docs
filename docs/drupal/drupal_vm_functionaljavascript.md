# Functional Javascript Testing

#### 1 Requirements

Make sure you have correctly set your `phpunit.xml`, see [Integrate PHPUnit in PhpStorm](drupal_vm_phpunit.md#1-integrate-phpunit-in-phpstorm).

#### 2. Install PhantomJS

The tests are run on a headless browser called `PhantomJS`. The first step is to install `PhantomJS` on your computer.

Open the vagrant machine `config.yml` file and add `phantomjs` to `extra_packages`:

    extra_packages:
      ...
      - phantomjs

From your `terminal` go on the `vagrant` directory and run `vagrant up --provision`, to apply the changes on your `vagrant` machine, or run `vagrant provision` if your machine is already up.

Enter in your `vagrant` machine (`vagrant ssh`) and run:

    export QT_QPA_PLATFORM=offscreen

#### 3. Run Tests

Once you've got `PhantomJS` you need start it to run your tests.

From your vagrant machine move to your `drupal` webroot (i.e. `/var/www/drupaltestvm/drupal/web`) and run:

    phantomjs --ssl-protocol=any --ignore-ssl-errors=true vendor/jcalderonzumba/gastonjs/src/Client/main.js 8510 1024 768

Now, if everything is fine, `PhantomJS` is up and running. Leave this terminal open and move to a new terminal to run your tests.

Let's run an example test to check everything works fine.

    # move to your drupal web root
    cd /var/www/drupaltestvm/drupal/web
    # run test
    php ./core/scripts/run-tests.sh --sqlite /tmp/test.sqlite --file core/modules/views/tests/src/FunctionalJavascript/ClickSortingAJAXTest.php