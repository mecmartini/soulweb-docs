# Responsive Testing with Galen

[Galen](http://galenframework.com/) is an open source layout testing tool for software applications, which helps us test the look and feel of the application.

#### 1. Requirements

Make sure you have `Nvm` and `Node.js` installed on your `vagrant` machine. See [Install Node.js using Nvm](drupal_vm_eslint.md#1-install-nodejs-using-nvm).

Open the vagrant machine `config.yml` file and make sure to have the `java` line uncommented on `installed_extras`

    installed_extras:
          ...
          - java
          ...

From your `terminal` go on the `vagrant` directory and run `vagrant up --provision`, to apply the changes on your `vagrant` machine, or run `vagrant provision` if your machine is already up.

Make sure you have the `Selenium Server` requirements on your host machine. See [Behat Requirements](drupal_vm_behat.md#1-requirements).

#### 2. Install Galen Framework

From your `vagrant` machine run:

    sudo npm install -g galenframework-cli

#### 3. Setup Galen

From your `vagrant` machine, on your `drupal` project folder, the following steps will help you get your first Galen tests up and running!

If your project docroot already have the folder `tests/ReponsiveTests` in your `drupal` docroot and it contains the files `config.js` `galen.config` `package.json` then skip this and jump directly to [Install Galen dependencies](drupal_vm_galen.md#3-install-galen-dependencies).

Create the folder `tests/ReponsiveTests/` under your docroot, and in this folder create the file `galen.config` which would contain the configuration parameters for Galen:

    # Range approximation
    # ~~~~~~~~~~~~~~~~~~~~~
    # Defines the approximation value for ranges when using "~" in galen page specs
    # This value means how many pixels or percents should it take constructing a range
    # e.g. if we define approximation as 5 then the following spec:
    #   height: ~ 50 px
    # will actually be replaced by Galen with this:
    #   height: 45 to 55px
    galen.range.approximation=2


    # Custom Listeners
    # ~~~~~~~~~~~~~~~~~~~~~~~
    # A comma separated list of class paths to reporting listeners
    # The defined listeners will be picked up by Galen and used for reporting
    #
    # galen.reporting.listeners=


    # Using page urls from last checked page in HTML report
    # ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    # This property enables the use of last page url in a page test
    # Needed when for some pages on the website there is no way to open it by direct url
    # galen.reporting.html.useLastPageUrls = true

    # Default browser
    # ~~~~~~~~~~~~~~~~~~~~~~~~
    # A browser that should be used by default in case it was not specified in galen test
    galen.default.browser=firefox



    # Color scheme spec precision
    # ~~~~~~~~~~~~~~~~~~~~~~~~
    # A value between 8 and 256 for color spectrum accuracy.
    spec.colorscheme.precision = 256


    # Full screenshots
    # ~~~~~~~~~~~~~~~~~~~~
    # In some browsers it is not possible to create a complete screenshot of whole page.
    # With this property enabled Galen will scroll page and make screenshots of parts of it.
    # Then it will assemble it in a one big screenshot
    #
    galen.browser.screenshots.fullPage = false
    # the following parameter is need in case the upper parameter is set to true
    # it sets the amount of time in milliseconds needed for a check that the page was scrolled when taking full page screenshots
    galen.browser.screenshots.fullPage.scrollWait = 0



    # Color scheme spec test color range
    # ~~~~~~~~~~~~~~~~~~~~~~~~~
    # A value between 0 and 256 which defined the range of nearby colors
    # in spectrum which will be picked up for calculating the percentage of usage
    spec.colorscheme.testrange = 6



    # Running in Selenium Grid
    # ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    # You can run your tests in Selenium Grid without modifying the tests.
    # Just enable the "galen.browserFactory.selenium.runInGrid" property
    # and Galen will always choose a Selenium Grid instead of running tests against local browsers
    # Also make sure you provide the proper url to grid
    #
    galen.browserFactory.selenium.runInGrid = true
    galen.browserFactory.selenium.grid.url = http://10.0.2.2:4444/wd/hub
    galen.browserFactory.selenium.grid.browser = chrome
    # galen.browserFactory.selenium.grid.browserVersion =
    # galen.browserFactory.selenium.grid.platform =


    # Exit with fail code in case of any failures
    # ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    # galen.use.fail.exit.code = true


    # Test file extension for standard test runner
    # ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    galen.test.suffix=.test


    # JavaScript Test file extension for JavaScript test runner
    # ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    galen.test.js.file.suffix=.test.js



    # Area finder for page elements
    # ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    # Defines a method of extracting location and size of web element. By default 'native' is used, which means
    # that Galen will use WebElement.getLocation() and WebElement.getSize() Java methods to construct area of page element.
    # But on some devices (e.g. iPhone with iOS 8.0) this would not work and there you need to switch to another method.
    # All possible methods
    # - native - Uses WebDriver methods for getting location and size
    # - jsbased - Uses JavaScript getBoundingClientRect() function in order to get area for page element.
    # - jsbased_native - Uses 'jsbased' method but in case of error goes back to 'native' method
    # - custom - Uses user-defined JavaScript for getting area of page element. If you use this method, you need to also
    #            provide a script via galen.browser.pageElement.areaFinder.custom.script property
    galen.browser.pageElement.areaFinder = native

Create also the file `package.json`, to install the [galen-framework-handler](https://github.com/agomezmoron/galen-framework-handler) with this content:

    {
      "dependencies": {
        "galen-framework-handler": "^0.0.9"
      },
      "devDependencies": {
      },
      "engines": {
        "node": ">=5.5.0"
      },
      "keywords": [
        "galen",
        "galen framework",
        "responsive",
        "testing",
        "DrupalDevDays"
      ],
      "scripts": {}
    }

And, at the end, create also the file `config.js` that contains:

    /** Overriding the config variable defined in the galen-framework-handler **/
    config = {
      baseURL: "http://drupalvm.dev"
    };

    /** Overriding the devices to test variable defined in the galen-framework-handler **/
    devicesToTest = {
      iphone7: devices.iphone7,
      ipadMini3: devices.ipadMini3,
      desktop: devices.desktop1024
    };

Edit the `baseURL` parameter with your host URL.

#### 4. Install Galen dependencies

From your `vagrant` machine, in the folder `tests/ReponsiveTests/` on your `drupal` docroot, run:

    npm install

#### 5. Run your first Galen test

In your project docroot, under `tests/ReponsiveTests`, create the folder `specs`.

Create file 'test-index.gspec' in `specs/` folder. Edit the file and add this code:

    @objects
       # menu items
       main-content xpath   //main[@id='content']

    = Main content =
       # for all the devices
       @on *
           main-content:
               visible

Under `tests/ReponsiveTests` create the folder `tests/suites/`.

Create file `test-index.test.js` in `tests/suites/` folder. Edit the file and add this code:

    // important, commons should be loaded here
    load('../../node_modules/galen-framework-handler/dist/galen-framework-handler.js');

    load('../../config.js');

    forAll(devicesToTest, function (device) {
      test("Testing on ${deviceName}", function (device) {

        // here goes a test code
        var driver = createDriver(config.baseURL, device.size);

        // here is the "key" of the galen-framework testing
        checkLayout(driver, "specs/uew-index.gspec", device.tags);

        driver.close();
        driver.quit();
      });
    });

You are ready to run your first test using `Galen`.

From your host machine, move to the folder where you previously placed the `Selenium Server` (see [Galen Requirements](drupal_vm_galen.md#1-requirements)) and run it up:

    cd /my/selenium/server/standalone/folder
    java -jar selenium-server-standalone-3.4.0.jar

From your `vagrant` machine under `docroot/tests/ReponsiveTests` run:

    galen test tests/suites/* --htmlreport reports/

If everything worked out, you’ll see `Selenium` open up a new instance of the selected browser profile and drive it through the test suites.

#### 6. Git ignore Galen package files

Edit the main `.gitignore` file to add the `Galen` package files.

The files/folders to ignore are:

* Folder `/tests/ReponsiveTests/node_modules/`
* Folder `/tests/ReponsiveTests/reports/`

#### 7. Cross-Browser test with Galen

In our `galen.config` selenium is setup to run with `chrome` browser.

To test on multiple browser we need to setup a new config file for each browser.

For example, if we want to setup tests on `firefox` browser, from your `vagrant` machine under `docroot/tests/ReponsiveTests` run:

    cp galen.config galenfirefox.config

Then edit the created file `galenfirefox.config` and change the `galen.browserFactory.selenium.grid.browser` parameter as below:

    galen.browserFactory.selenium.grid.browser = firefox

Now to run your test:

    galen test tests/suites/* --htmlreport reports/ --config galenfirefox.config