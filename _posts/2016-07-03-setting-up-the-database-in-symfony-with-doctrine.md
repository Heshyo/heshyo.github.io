---
layout: post
title: Setting up the database in Symfony with Doctrine
---

Doctrine is the default [ORM](https://en.wikipedia.org/wiki/Object-relational_mapping) of Symfony. It's very simple to create the database, entities (the PHP objects) as well as the corresponding tables.

Configuration
-------------

When installing Symfony you must have set the parameters for the database. You can always check `/app/config/parameters.xml` if you need to change them.

To make sure your database is using UTF8, go to `app/config/config.yml` and add or modify the following:

```yml
# app/config/config.yml
doctrine:
    dbal:
        charset: utf8mb4
        default_table_options:
            charset: utf8mb4
            collate: utf8mb4_unicode_ci
```

If you're using SQLite, you'll see some recommandation in that file.

Create the database
-------------------

To create the database, simply run the following command:

```
php bin/console doctrine:database:create
```

Create the entities
-------------------

Entities are the objects that you'll be using in PHP and which will have matching tables in the database.

You can either manually create the entity, or use the following interactive command:

```
php bin/console doctrine:generate:entity
```

If you run the command, you'll be asked for

- the configuration format to use. By default it's _annotation_, meaning that you'll add some info for doctrine through comments straight in the php file of your entity. That's what I'm using.
- the name of the entity, of the form NamespaceOfYourBundle:NameOfYourEntity, eg `AppBundle:Article`
- each and every column you want to add. You'll enter first
	- column name. Note that you should avoid reserved SQL words like _GROUP_, _USER_, _NUMBER_. If for some reasons you have to using reserved words, it will require some [additional work](http://docs.doctrine-project.org/projects/doctrine-orm/en/latest/reference/basic-mapping.html#quoting-reserved-words).
	- column type (string by default)
	- is nullable (false by default)
	- is unique (false by default)

Once you're done, Doctrine will have create the corresponding php file, eg `src/AppBundle/Entity/Article.php`. You can of course tweak its content afterwards. 

By default, an _id_ column is created. Its value will be an automatically generated integer, and it will be the key of the table. If for some reasons you want to provide the id yourself, you can simply remove the annotation (if you've chosen the _annotation_ format) `@ORM\GeneratedValue(strategy="AUTO")`, or replace `AUTO` with `NONE`.

Generate the getters and setters of your entitie(s)
---------------------------------------------------

If you take a look at the generated entity file, you'll probably notice that it contains for each column a corresponding field, getter method and setter method. If you add a new field manually that correspond to a column in the table, you'll have to add the corresponding getter and setter. This can be done by running the following command

```

# generate the entity "Article" in "AppBundle"
php bin/console doctrine:generate:entities AppBundle/Entity/Article

# generates all entities in "AppBundle"
php bin/console doctrine:generate:entities AppBundle

# generates all entities of bundles in the "Acme" namespace
php bin/console doctrine:generate:entities Acme

```

This command is safe to run and rerun. It will only add necessary getters and setters. It will also by default create a backup next to the original file, that ends with ~, eg `Article.php~`. If you get a `Cannot redeclare class` error, make sure to either remove that backup or use the `--no-backup` option when running the previous command.

Generating the table
--------------------

Thanks to the entities you created and the metadata you added (eg through _annotations_), Doctrine knows what your tables look like. You can now run the following command to create all your tables automatically:

```
php bin/console doctrine:schema:update --force
```

Note that this command is not just for creating tables, but also to update them! So if you create your table, then decide to add a new field in your entity, then run the command again, it will `ALTER` your current table. You may want to first check what the SQL command look like by running

```
php bin/console doctrine:schema:update --dump-sql
```

If you're happy with it, just run it again with `--force`.

You probably won't want to run those commands on the live server, unless the modifications are very simple, like just adding new tables. For more complex tasks you can use the [DoctrineMigrationBundle](https://symfony.com/doc/current/bundles/DoctrineMigrationsBundle/index.html).