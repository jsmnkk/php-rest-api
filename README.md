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

    require __DIR__ . '/../vendor/autoload.php';

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

# To Deploy REST API via Heroku

- **Step 1**: Now add a _Procfile_ in your project with the contents:-

  ```Procfile
  web: vendor/bin/heroku-php-apache2 public/
  ```

- **Step 2**: Since we are using Apache, we need to setup a _.htaccess_ file in _public_ to route requests to _index.php_. So create one with contents:-

  ```htaccess
  RewriteEngine On
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteRule ^ index.php [QSA,L]
  ```

- **Step 3**: Now create a folder _.github/workflows_ and in there create a file _main.yml_ with contents:

  ```yaml
  name: Deploy
  on:
    push:
      branches:
        - master
  jobs:
    build:
      runs-on: ubuntu-latest

      steps:
        - uses: actions/checkout@v1.0.0
        - uses: akhileshns/heroku-deploy@master
          with:
            heroku_api_key: ${{secrets.HEROKU_API_KEY}}
            heroku_email: ${{secrets.HEROKU_EMAIL}}
            heroku_app_name: ${{secrets.HEROKU_APP_NAME}}
  ```

- **Step 4**: Now we can push this to GitHub but before that, make sure you have created a Heroku account and in account settings, copy the api key. Then in the github repo for this project, go to settings and add secrets HEROKU_API_KEY (Your copied apikey), HEROKU EMAIL (The email associated with your heroku account) and HEROKU_APP_NAME (The name of your app and keep in mind it needs to be unique in heroku)

  Now whenever you push to the master branch of your github repo, your app is automatically deployed to heroku
