# BDD with Behat

[Behat](http://behat.org/) is an open source `behavior-driven development` (`BDD`) tool for `PHP`. You can use `Behat` to build and run automated tests for site functionality on your `Drupal` sites.

#### 1. Requirements

From your `vagrant` machine on your `drupal` project folder, you must add the `Drupal` extension for `Behat`:

    composer require --dev drupal/drupal-extension

[Selenium2](http://www.seleniumhq.org/) is needed to test `javascript` session. `Selenium2` gives you the ability to take full control of a real browser with a clean consistent proxy API.

First ensure that you have the `Java Runtime Environment` (`JRE`) installed on your host machine, which is required to run Selenium. You may already have it installed, but can check with:

    java -version

If you need to install or update, Google is your friend.

From your host download the `Selenium Server` and place it wherever you like:

    # check for the latest version at http://docs.seleniumhq.org/download/
    curl -O http://selenium-release.storage.googleapis.com/3.4/selenium-server-standalone-3.4.0.jar
    mv selenium-server-standalone-3.4.0.jar /wherever/you/like/folder

We also need to download the browser `drivers` to make `Selenium` deal with them:

    # download chrome driver
    # check for the latest version at https://sites.google.com/a/chromium.org/chromedriver/downloads
    curl -L -O https://chromedriver.storage.googleapis.com/2.30/chromedriver_mac64.zip
    # uncompress it
    unzip chromedriver_mac64.zip
    # move it on exec folder and give exec permission
    chmod +x chromedriver
    sudo mv chromedriver /usr/local/bin

    # download firefox driver
    # check for the latest version at https://github.com/mozilla/geckodriver/releases
    curl -L -O https://github.com/mozilla/geckodriver/releases/download/v0.18.0/geckodriver-v0.18.0-macos.tar.gz
    # uncompress it
    tar -zxvf geckodriver-v0.18.0-macos.tar.gz
    # move it on exec folder and give exec permission
    chmod +x geckodriver
    sudo mv geckodriver /usr/local/bin

#### 2. Setup Behat

From your `vagrant` machine, on your `drupal` project folder, the following steps will help you get your first Behat tests up and running!

If your project already have the file `behat.yml` in your `drupal` docroot then skip this and jump directly to [Run your first Behat test](drupal_vm_behat.md#3-run-your-first-behat-test).

Create a `behat.yml` file inside the docroot of your site (e.g. create this file alongside the rest of the `Drupal` codebase at `/var/www/drupalvm/drupal/behat.yml`), with the following contents:

    chrome:
      autoload:
        '': %paths.base%/features/bootstrap
      suites:
        web_features:
          paths: [ %paths.base%/features/web ]
          contexts:
            - WebContext
            - Drupal\DrupalExtension\Context\DrupalContext
            - Drupal\DrupalExtension\Context\MinkContext
            - Drupal\DrupalExtension\Context\MessageContext
            - Drupal\DrupalExtension\Context\DrushContext
      extensions:
        Behat\MinkExtension:
          goutte: ~
          javascript_session: 'selenium2'
          selenium2:
            wd_host: http://10.0.2.2:4444/wd/hub
            browser: 'chrome'
          base_url: http://drupalvm.dev
          browser_name: 'chrome'
        Drupal\DrupalExtension:
          blackbox: ~
          api_driver: 'drupal'
          drupal:
            drupal_root: '/var/www/drupalvm/drupal/web'
          region_map:
            content: "#content"

    firefox:
      autoload:
        '': %paths.base%/features/bootstrap
      suites:
        web_features:
          paths: [ %paths.base%/features/web ]
          contexts:
            - WebContext
            - Drupal\DrupalExtension\Context\DrupalContext
            - Drupal\DrupalExtension\Context\MinkContext
            - Drupal\DrupalExtension\Context\MessageContext
            - Drupal\DrupalExtension\Context\DrushContext
      extensions:
        Behat\MinkExtension:
          goutte: ~
          javascript_session: 'selenium2'
          selenium2:
            wd_host: http://10.0.2.2:4444/wd/hub
            browser: 'firefox'
          base_url: http://drupalvm.dev
          browser_name: 'firefox'
        Drupal\DrupalExtension:
          blackbox: ~
          api_driver: 'drupal'
          drupal:
            drupal_root: '/var/www/drupalvm/drupal/web'
          region_map:
            content: "#content"

    safari:
      autoload:
        '': %paths.base%/features/bootstrap
      suites:
        web_features:
          paths: [ %paths.base%/features/web ]
          contexts:
            - WebContext
            - Drupal\DrupalExtension\Context\DrupalContext
            - Drupal\DrupalExtension\Context\MinkContext
            - Drupal\DrupalExtension\Context\MessageContext
            - Drupal\DrupalExtension\Context\DrushContext
      extensions:
        Behat\MinkExtension:
          goutte: ~
          javascript_session: 'selenium2'
          selenium2:
            wd_host: http://10.0.2.2:4444/wd/hub
            browser: 'safari'
          base_url: http://drupalvm.dev
          browser_name: 'safari'
        Drupal\DrupalExtension:
          blackbox: ~
          api_driver: 'drupal'
          drupal:
            drupal_root: '/var/www/drupalvm/drupal/web'
          region_map:
            content: "#content"

and edit the `base_url` and `drupal_root` parameters of the three profile (`chrome` - `firefox` - `safari`) with your parameters.

To initialize the `Behat` features folder, where you will place test cases, you must run:

    ./vendor/bin/behat --init

the `features` folder must be created on your `Drupal` docroot.

#### 3. Run your first Behat test

Open up the new `features/web` folder `Behat` just created. Inside that folder, create `HomeContent.feature` file with the following contents:

    Feature: Test DrupalContext
      In order to prove Behat is working correctly in Drupal VM
      As a developer
      I need to run a simple interface test

      @javascript
      Scenario: Viewing content in a region
        Given I am on the homepage
        Then I should see "No front page content has been created yet" in the "content"

!!! note "Note"
    The `@javascript` is needed to run `javascript` session, otherwise the tests will run in a `headless` browser.

From your host machine, move to the folder where you previously placed the `Selenium Server` and run it up:

    cd /my/selenium/server/standalone/folder
    java -jar selenium-server-standalone-3.4.0.jar

Now you can finally run your `Behat` test and see the browser in action. From your `vagrant` machine on `drupal` docroot folder run:

    # to run test on chrome
    ./vendor/bin/behat -v -c behat.yml -p chrome

    # to run test on firefox
    ./vendor/bin/behat -v -c behat.yml -p firefox

    # to run test on safari
    ./vendor/bin/behat -v -c behat.yml -p safari

!!! note "Run with Safari"
    To make it works with `safari` browser you must enable, from `safari` browser, the `Allow Remote Automation` under `Develop` menu.

If everything worked out, you’ll see `Selenium` open up a new instance of the selected browser profile and drive it through the test suites.