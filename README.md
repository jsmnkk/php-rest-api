# JS REST API

- **Step 1**: To install PHP and Composer run

  - windows - `choco install php composer -y`
  - macos - `brew install php composer`
  - linux -

  ```bash
  # Run these commands one after another
  sudo apt install php

  sudo apt update && sudo apt install wget php-cli php-zip unzip curl
  curl -sS https://getcomposer.org/installer |php
  sudo mv composer.phar /usr/local/bin/composer
  ```

  [OR] Download and install PHP from [here](https://www.php.net/downloads.php) and Composer from [here](https://getcomposer.org/download/)

  Verify everything is installed using `php --version` and `composer --version`

- **Step 2**: Create a new folder, inside this folder create another folder called _public_ and place an _index.php_ file in there with the contents:-

  ```php
  <?php
    use Psr\Http\Message\ResponseInterface as Response;
    use Psr\Http\Message\ServerRequestInterface as Request;
    use Slim\Factory\AppFactory;

    require('vendor/autoload.php');

    $app = AppFactory::create();

    $app->get('/hello', function (Request $request, Response $response, $args) {
        $response->getBody()->write("world of php");
        return $response;
    });

    $app->run();
  ?>
  ```

- **Step 3**: In your project folder run `composer require slim/slim:"4.*"` and `composer require slim/psr7`

- **Step 4**: Finally run `php -S 127.0.0.1:5000 -t public public/index.php` to launch your REST API

- **Step 6**: Don't forget to add a _.gitignore_ file and use git to manage your project. The contents of the _.gitignore_ file are:-
  ```
  /vendor
  .DS_Store
  .env
  ```
