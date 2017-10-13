---
layout: post
title: Adding a RESTful API to Symfony
---

In the [previous post][prev], we set up Symfony. Let's start creating a RESTful API. We'll make use of the [FOSRestTBundle](http://symfony.com/doc/current/bundles/FOSRestBundle/index.html). FYI, a lot of Symfony bundles have name starting with FOS, it means Friends Of Symfony.

Manually adding REST
--------------------

As mentioned in the [previous post][prev], there's a REST edition of Symfony, but at the time of writing it was based on an earlier version of Symfony. We can instead install the Symfony standard edition and manually add the necessary bundles:

```
composer require friendsofsymfony/rest-bundle
composer require jms/serializer-bundle
```

The 1st bundle is what enables to easily create a RESTful API, the 2nd one is what will be used to [serialize](https://en.wikipedia.org/wiki/Serialization) what is returned by the RESTful API.

Then in `AppKernel.php` you need to register those 2 bundles so that they will actually be used. Simply add under `AppKernel::registerBundles()`

```php
$bundles = array(
    // ...list of already loaded bundles...
    new FOS\RestBundle\FOSRestBundle(),
    new JMS\SerializerBundle\JMSSerializerBundle(),
);
```

If you realize you are not using some bundles from the standard edition, you can simply remove them from the array.

Configuring REST
----------------

You'll find all the configuration options in the [FOSRestBundle configuration options page](http://symfony.com/doc/current/bundles/FOSRestBundle/configuration-reference.html). It lists all the default options (meaning that if you want the same value, there's no need to add it in your own configuration file).

As mentioned in the [previous post][prev], you'll find all the configuration files in the `app/config/` folder. You'll see

- config.yml
- config_dev.yml
- config_prod.yml
- config_test.yml

`config.yml` is the base configuration for Symfony, the other 3 files are variants for the different environments: dev, prod and test. If you look at those 3 files, you'll see they all include `config.yml` by default, so that there's no need to duplicate things between the different configurations. You'll also see the the dev configuration contains the `web_profiler`, while the prod configuration does not. That's why when using `app_dev.php`, you see the web profiler bar.

Here we'll want to configure REST for everyone, so we'll modify `config.yml`. The `yml` extension stands for [YAML](https://en.wikipedia.org/wiki/YAML). It's pretty straight forward to understand the format. To make sure Symfony interprets your YAML files correctly, make sure you use 4 spaces for indenting each level. You may find that things don't work well if you start using tabs and/or different indentation for elements that should be at the same level. It's better to make sure your text editor shows spaces. For now, I'll just add

```yaml
fos_rest:
    # If you want to have "ParamFetcher $paramFetcher" accessible in your function
    param_fetcher_listener: true
    routing_loader:
        default_format: json
        include_format: false
```

When doing a query to your RESTful API, it has the possibilit to return the data in different formats. Here is specified `default_format: json` because I always want to fallback to JSON, and `include_format` because I always want to get JSON, so I don't want to have to specify the format in the query I do. If you want to handle several formats, just leave `include_format` alone as it's `true` by default, then when doing a query simply put the format as extension, eg `/api/user/123.json` or `/api/user/123.xml`.

By default you shoud have `DefaultController.php` under `src/AppBundle/Controller`. You can create a new controller called `RestController.php` and add the following in `app/config/routing.xml`:

```yml
api:
    type:     rest
    resource: AppBundle\Controller\RestController
    prefix:   /api
```

This simply says that all routes starting with `/api` (the `prefix`) are of type `rest` (meaning they'll be using the FOSRestBundle) and the corresponding controller to use is `RestController.php`. Now let's write a sample `RestController.php`:

```php
<?php

namespace AppBundle\Controller;

use FOS\RestBundle\Controller as FOSCtlr;

class UsersController extends FOSCtlr\FOSRestController
{
    public function getUsersAction()
    {} // "get_users"            [GET] /users

    public function postUsersAction()
    {} // "post_users"           [POST] /users

    public function getUserAction($slug)
    {} // "get_user"             [GET] /users/{slug}

    public function putUserAction($slug)
    {} // "put_user"             [PUT] /users/{slug}

    public function deleteUserAction($slug)
    {} // "delete_user"          [DELETE] /users/{slug}

    public function getUserCommentsAction($slug)
    {} // "get_user_comments"    [GET] /users/{slug}/comments

    public function getUserCommentAction($slug, $id)
    {} // "get_user_comment"     [GET] /users/{slug}/comments/{id}

    public function putUserCommentAction($slug, $id)
    {} // "put_user_comment"     [PUT] /users/{slug}/comments/{id}
}
```

You will notice that all the public functions from the controller

- start with an HTML verb (we're doing [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) remember) like get, post, put, delete
- end with "Action" (the casing is important)

By following that simple rule, FOSRestBundle will automatically generate routes for you, and will ensure that those routes are accessed correctly (eg a _get_ route is indeed accessed through an HTTP GET).

Between the HTTP verb and the "Action" word, you can have one or more words, using [CamelCase](https://en.wikipedia.org/wiki/CamelCase). The route created by FOSRestBundle will be based on those words, eg

```php
public function getUsersAction() {} // it's "users" plural
```

will create the route `/users`, accessible through GET (as in the `routing.yml` file we configured our controller to work on routes starting with `api`, the full route will be `/api/users`). Notice the ending 's' to the function and route. It's a parameterless route, and the function is expected to return all the users. Compare it to

```php
public function getUserAction($userId) {} // it's "user" singular
```

will create the route `/users/{userId}`, accessible through GET, that takes a parameter `userId`, so an actual route could be `/users/123`, to get the user with the ID 123. Notice that there's no ending 's' on the function, but there is one in the route. FOSRestBundle has a certain knowledge on singular and plural, so you may need to be careful when creating your functions.

```php
    public function postUsersAction() {}
```

will create the route `/users`, accessible through POST. The posted data will be accessible by the function (we'll see more on this later).

Here are 2 more examples

```php
    public function getUserCommentsAction($userId) {}
    public function getUserCommentAction($userId, $commentId) {}
```

Those 2 functions are pretty similar and have almost the same name, but they do different things

- Both have 2 words between the verb and "Action", but with and without an 's' (that's why it's important to try to allways follow a specific pattern with the 's', and that's why FOSRestBundle needs to understand plural and singular).
- The 1st one has only one parameter, meaning it's linked to the 1st word
- The 2nd one has 2 parameters, meaning the 1st one is linked to the 1st word, the 2nd one to the 2nd word

Eventually we end up with the following routes:

- `/users/{$userId}/comments` that's supposed to get all the comments of a specific user
- `/users/{$userId}/comments/{$commentId}` that's supposed to get a specific comment of a specific user

It can be a bit confusing at 1st to correctly name the function, but you get use to it quite fast.

You can check the [routing](http://symfony.com/doc/current/bundles/FOSRestBundle/5-automatic-route-generation_single-restful-controller.html) for single RESTful controller for more info. You'll see that you have access to [additional verbs](http://symfony.com/doc/current/bundles/FOSRestBundle/5-automatic-route-generation_single-restful-controller.html#rest-actions) and [actions](http://symfony.com/doc/current/bundles/FOSRestBundle/5-automatic-route-generation_single-restful-controller.html#conventional-actions), as well as disable pluralization of routes, or even [define your own plurals](http://symfony.com/doc/current/bundles/FOSRestBundle/5-automatic-route-generation_single-restful-controller.html#changing-pluralization-in-generated-routes).

To make sure you wrote the name of your functions correctly, run the following command to see the list of all the routes that you have (note that you'll also see some routes defined by other bundles, like the profiler):
```
php bin/console debug:router
```

[prev]:/2016/07/01/setting-up-symfony.html