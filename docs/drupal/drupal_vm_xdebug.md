# Xdebug

#### 1. Enable Xdebug on your Vagrant machine

Open the vagrant machine `config.yml` file and be sure to have the xdebug line uncommented on `installed_extras`

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

Be sure to have the following lines set as:

    # XDebug configuration. XDebug is disabled by default for better performance.
    php_xdebug_default_enable: 1
    php_xdebug_coverage_enable: 1
    
Add the port `9000` to `firewall_allowed_tcp_ports`

    firewall_allowed_tcp_ports:
      - "22"
      - "25"
      - "80"
      - "81"
      - "443"
      - "4444"
      - "8000"
      - "8025"
      - "8080"
      - "8443"
      - "8983"
      - "9000"
      - "9200"
      
Run `vagrant up --provision` to apply the changes on your `vagrant` machine or run `vagrant provision` if your machine is already up.

#### 2. Integrate Xdebug on PhpStorm

