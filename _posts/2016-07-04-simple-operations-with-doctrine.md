---
layout: post
title: Simple operations with Doctrine
---


CRUD with Doctrine
------------------

Let's say you created an `Article` entity. We'll see how we can Create, Read, Update and Delete rows from the corresponding `article` table.

### Create

```php
<?php

public function postArticleAction(Request $request) {
    $post = $request->request;

    // Create your object and sets its properties. No need to set its ID,
    // unless you specified you don't want the ID to be automatically created.
    $article = new Article();
    $article->setTitle($post->get('title'));
    $article->setContent($post->get('content'));

    // Get the Entity Manager
    $em = $this->getDoctrine()->getManager();

    // Tell Doctrine you will want to save the article.
    // Doctrine will just queue that query, it won't actually execute it.
    $em->persist($article);

    // Tells Doctrine to execute all the queries that have been queued.
    $em->flush();

    // If the ID is automatically created, then you'll have access to it
    // after the query is executed.

    // Returns the created article with an HTTP 200 (ie OK) response
    $view = $this->view($article, 200);
    return $this->handleView($view);
}
```

### Read

```php
<?php

public function getArticleAction($articleId) {
    // Gets the article by ID.
    $article = $this->getDoctrine()
        ->getRepository('AppBundle:Article')
        ->find($articleId);

    if (!$article) {
        // Returns a 404 if we can't find the specified article.
        $view = $this->view("No article found for id $articleId", 404);
        return $this->handleView($view);
    }

    $view = $this->view($article, 200);
    return $this->handleView($view);
}
```

### Update

```php
<?php

public function putArticleAction($articleId, Request $request) {
    $post = $request->request;
    $em = $this->getDoctrine()->getManager();
    $article = $em->getRepository('AppBundle:Article')->find($articleId);

    if (!$article) {
        // Returns a 404 if we can't find the specified article.
        $view = $this->view("No article found for id $articleId", 404);
        return $this->handleView($view);
    }

    $article->setTitle($post->get('title'));
    $article->setContent($post->get('content'));
    $em->flush();

    $view = $this->view($article, 200);
    return $this->handleView($view);
}
```

### Delete

```php
<?php

public function deleteArticleAction($articleId) {
    $em = $this->getDoctrine()->getManager();
    $article = $em->getRepository('AppBundle:Article')->find($articleId);

    if (!$article) {
        // Returns a 404 if we can't find the specified article.
        $view = $this->view("No article found for id $articleId", 404);
        return $this->handleView($view);
    }

    $em->remove($article);
    $em->flush();

    $view = $this->view(true, 200);
    return $this->handleView($view);
}
```

What's next
-----------

You've seen how to create the database, create entities and their associated tables, as well as do CRUD commands on them. But currently, all the entities are separated, meaning there's no relationship between them. I'll cover this next time.

Check the [doctrine page](http://symfony.com/doc/current/book/doctrine.html) if you need more info.