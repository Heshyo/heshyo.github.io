---
layout: post
title: Setting up Symfony
---

Symfony?
--------

[Symfony](https://symfony.com) is a PHP framework, like [Laravel](https://laravel.com/), [CodeIgniter](https://codeigniter.com/), [CakePHP](http://cakephp.org/) and countless others. Instead of being a monolithic framework, it's composed of lots of bricks, called _Bundles_. There are already lots of such bundles available, for example to handle the database, users, REST API, ...

2 ways to get Symfony
---------------------

### With the Symfony installer

```
php -r "readfile('https://symfony.com/installer');" > symfony
```

Then 

```
php symfony new my_project_name
```


### Or using Composer
Using the installer didn't work for me as cURL was complaining about some certificates that couldn't be validated. So I used [Composer](https://getcomposer.org/) (a package manager for PHP, like [npm](https://docs.npmjs.com/) for Javascript).

```
composer create-project symfony/framework-standard-edition my_project_name
```

### Don't want the standard edition?
There are several editions or packages of Symfony. Eg if you're only interested in creating a REST API, you can try using the REST edition

```
composer create-project gimler/symfony-rest-edition --stability=dev my_project_name
```

Note that at the time of writing, the standard edition was using Symfony 3.1.2 while the REST version was using Symfony 2.7.14. I decided to keep the standard edition and manually add the [REST bundle](https://symfony.com/doc/current/bundles/FOSRestBundle/index.html).

For more info on how to install Symfony, check the [installation page](https://symfony.com/doc/current/book/installation.html).

Initial configuration of Symfony
--------------------------------

You'll be asked to enter some parameters for your database as well as mail server. Default values are specified I you don't enter anything, and you can modify those parameters anytime later on.

Let's run Symfony
-----------------

Symfony comes with several commands available under bin/console. To start a web server for your new project, just go to your project directory and run

```
php bin/console server:run
```

Then browse to the address displayed by the previous command (eg `http://127.0.0.1:8000`) and you should see the welcome page.

Checking the installation
------------------------

Now it's time to open the `config.php` file (eg `http://127.0.0.1:8000/config.php`) to make sure everything is fine. Symfony will display recommandations or issues with the current installation.

Structure of Symfony
--------------------

### app folder
This contains several sub folders, mostly

- cache: that's where Symfony will cache things for the different environments (prod, dev and test). You will often have to clear the cache, using a specific command, when developing. As a rule of thumb, if you feel like something that should work does not work, clear the cache then try again!
- config: that's where the configuration files are
- logs: that's where by default Symfony will write its logs

### bin folder
This mostly contains the `console` file, which allows you to execute commands. It's like a [CLI](https://en.wikipedia.org/wiki/Command-line_interface), and bundles can add their own commands. For example you'll often have to run the following command to clear Symfony's cache:

```
php bin/console cache:clear
```

### src folder
That's where your code will be.

### vendor folder
That's where all the bundles that you install end up.

### web folder
This is the root of the web server meaning that all requests will go through it, so there's no way to access the previously mentioned folders. It contains only a few files:

- config.php: we already talked about it and you will probably only use it right after installing Symfony. It only works when accessed from localhost (if you read that file you'll see it makes sure of it)
- app.php: it's the script that will actually route all your requests. When setting up your webserver, this should be the entry point, so `http://127.0.0.1:8000/that/page` is the same as `http://127.0.0.1:8000/app.php/that/page`
- app_dev.php: same as app.php, except that it adds some debugging capabilities and nice error messages. If you're looking at `http://127.0.0.1:8000/that/page`, just go to `http://127.0.0.1:8000/app_dev.php/that/page`. Just like `config.php`, it's only available from localhost, for obvious security reasons.

Folder rights
-------------

The user running your webserver obviously needs read access to all the files, so if you run `composer install` under a different account, you may have to change the access rights to the `vendor` folder recursively. It also needs write access in a few folders, namely:

- /var/cache
- /var/logs
- /var/sessions