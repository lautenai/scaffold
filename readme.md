# Scaffold Laravel Bundle

Easily create scaffolds to manage your database.

## Installation

Install using the Artisan CLI:

	php artisan bundle:install scaffold
	php artisan bundle:publish

Then, place the bundle's name in your **application/bundles.php** file.

```php
<?php

return array(

	'scaffold',
);
```

Don't forget that each time you generate a scaffold, you will need to add the
Controller in your routes. Or, simply use this:

```php
<?php

Route::controller(Controller::detect());
```

## A Few Examples

### Creating a full Blog... in seconds

Say you want to make a blog that contains posts, which you want to be able
to create, edit, and delete. Each of these posts would have comments, which
would be posted by users. You could manually code all of that, or you
could just run:

	php artisan scaffold::make comment content:text belongs_to:post,user timestamps
	php artisan migrate

	php artisan scaffold::make post title:string content:text belongs_to:user has_many:comment timestamps
	php artisan migrate

	php artisan scaffold::make user username:string password:string has_many:post,comment
	php artisan migrate

Now isn't that a bit faster?

### Dissecting the Blog Example

Notice the basic structure of generating a scaffold:

	php artisan scaffold::make <table> <fields> <relationships> <timestamps>

`table`: The table's name in singular form

`fields`: The table's fields. Each field is separated by a space. The
different supported types are: string, integer, float, boolean, date,
timestamp, text, and blob. The general syntax for a field is:

	name:type
	
	title:string

Additionally, you can make a field nullable:

	name:type:nullable
	
	title:string:nullable

You can even set a maximum field size:

	name:type:length
	name:type:length:nullable
	
	title:string:255
	title:string:255:nullable

`relationships`: The relationships between this scaffold and other scaffolds.
Each different relationship is separated by a space. There different supported
relationships are: has_one, has_one_or_many, belongs_to, has_many, and
has_many_and_belongs_to. The general syntax for a relationship is:

	relationship:scaffold
	
	has_many:post

Note that the scaffold is **always** in its singular form. Additionally,
several similar relationships may be defined at the same time using a comma
(but no spaces):

	relationship:scaffold1,scaffold2
	
	has_many:post,comment

It is important to note that the different scaffolds listed in a single
relationship type should be ordered by importance. For example, a post
is more important than a comment.

`timestamps`: If included, this will make the scaffold automatically timestamp
when rows are created or updated. If `timestamps` is omitted, the scaffold
will not do this.

**Note**: Don't forget to run your migrations after you create a new scaffold!

### Using an existing Table to create a Scaffolding

Note: This is not implemented yet.

To create a new scaffold on an already existing table, simply run:

	php artisan scaffold::make post

Notice that the singular form of the table name is given.