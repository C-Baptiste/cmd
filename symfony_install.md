# php
- sudo dnf info php
- dnf module list php
- dnf module enable php:8.2
- sudo dnf info php
- sudo dnf install php

#  composer
- sudo dnf install composer

#  symfony cli
- curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.rpm.sh' | sudo -E bash
- sudo dnf install symfony-cli

#  new project
- symfony new symfony01 --version="6.4.*" --webapp

#  display info
- php bin/console about

#  start local dev
- symfony server:start

# 
- symfony check:security

#  list cmd
- symfony console
- php bin/console

 - symfony console debug:router

#  composer require twig

 
 
#  install from source
- wget 
- ./configure --prefix=/home/...
- make
- make install

- wget https://www.php.net/distributions/php-8.3.9.tar.gz
- tar -xf php-8.3.9.tar.gz 
- export SQLITE_CFLAGS="-I/home/reverb/sqlite/usr/include"
- export SQLITE_LIBS="-L/home/reverb/sqlite/usr/lib -lsqlite3"
- ./configure --enable-debug --prefix=/home/reverb/php/php/ --with-openssl
- make -j4
- make TEST_PHP_ARGS=-j4 test
- make install
- export PATH=/home/reverb/php/php/bin/:$PATH

 
- wget https://www.sqlite.org/2024/sqlite-autoconf-3460000.tar.gz
- ar -xf sqlite-autoconf-3460000.tar.gz 
- ./configure --prefix=/home/reverb/sqlite/usr
- make
- make install
- export PATH=/home/reverb/sqlite/usr/bin/:$PATH

 


- [reverb@lab02 symfony]$ php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
- [reverb@lab02 symfony]$ php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
- Installer verified
- [reverb@lab02 symfony]$ mkdir composer
- [reverb@lab02 symfony]$ php composer-setup.php --install-dir=/home/reverb/symfony/composer
- 
- 
- wget https://get.symfony.com/cli/installer
- wget https://get.symfony.com/cli/installer -O - | bash
- export PATH="$HOME/.symfony5/bin:$PATH"
- 
- /home/reverb/symfony/composer/composer.phar create-project symfony/skeleton:"6.4.*" test01
- /home/reverb/symfony/composer/composer.phar require webapp
- 
- composer require --dev symfony/maker-bundle
- symfony console list make
- symfony console make:controller
- composer require --dev symfony/profiler-pack
- http://localhost:8000/_profiler/
- 
- composer require symfony/orm-pack
- symfony console doctrine:database:create
- symfony console make:entity
- symfony console make:migration
- symfony console doctrine:migrations:status
- symfony console doctrine:migrations:migrate
- symfony console doctrine:migration:migrate --help
- composer show
- composer require --dev orm-fixtures
- symfony console doctrine:fixtures:load
- // composer require sensio/framework-extra-bundle
- // composer require symfony/form
- // symfony console make:form


- symfony new symfony02 --version="6.4.*" --webapp
- cd symfony02
- composer require easycorp/easyadmin-bundle
- code .
- php bin/console make:admin:dashboard
- symfony console make:admin:crud
- php bin/console make:admin:crud
- composer show
- composer require symfony/orm-pack
- composer require --dev symfony/maker-bundle
- php bin/console doctrine:database:create
- symfony console make:entity
- symfony console make:migration
- symfony console doctrine:migrations:migrate
- symfony console make:admin:crud
- symfony console make:entity
- symfony console make:migration
- symfony console doctrine:migrations:migrate
- symfony console make:admin:crud
-