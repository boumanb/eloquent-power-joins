# Eloquent Joins with Extra Powers

<!-- [![Latest Version on Packagist](https://img.shields.io/packagist/v/kirschbaum-development/laravel-where-has-with-joins.svg?style=flat-square)](https://packagist.org/packages/kirschbaum-development/laravel-where-has-with-joins) -->
[![Actions Status](https://github.com/kirschbaum-development/laravel-where-has-with-joins/workflows/CI/badge.svg)](https://github.com/kirschbaum-development/laravel-where-has-with-joins/actions)
<!-- [![Quality Score](https://img.shields.io/scrutinizer/g/kirschbaum-development/laravel-where-has-with-joins.svg?style=flat-square)](https://scrutinizer-ci.com/g/kirschbaum-development/laravel-where-has-with-joins) -->
<!-- [![Total Downloads](https://img.shields.io/packagist/dt/kirschbaum-development/laravel-where-has-with-joins.svg?style=flat-square)](https://packagist.org/packages/kirschbaum-development/laravel-where-has-with-joins) -->

Joins are very useful in a lot of ways. If you are here, you probably know them. This package gives you some extra powers to make your joins more readable and make you write less code, hidding some implementation details from places they don't need to be exposed.

## Installation

You can install the package via composer:

```bash
composer require kirschbaum-development/eloquent-joins-with-extra-powers
```

## Usage

This package provide a few different methods you can use.

### Join Relationship

Let's say you have an `User` model, which has a `hasMany` relationship with the `Post` model. If you want to join the tables, you would usually write something like:

```php
User::select('users.*')->join('posts', 'posts.user_id', '=', 'users.id')->toSql();
// select users.* from users inner join "posts" on "posts"."user_id" = "users"."id"
```

This package provides you the `joinRelationship` method.

```php
User::select('users.*')->joinRelationship('posts')->toSql();
// select users.* from users inner join "posts" on "posts"."user_id" = "users"."id"
```

And you have the same results. In terms of code, you didn't save much, but you are now using the relationship between users and posts do join the tables. This means that you are now hiding how this relationship work behind the scenes (implementation details), you also don't need to change the code if the relationship type changes and you have a more readable and less overwhelming code.

But, **it gets better** when you need to **join nested relationships**. Let's say you also have a `hasMany` relationship between the `Post` and `Comment` models and you need to join these tables.

```php
User::select('users.*')->join('posts', 'posts.user_id', '=', 'users.id')->join('posts', 'posts.user_id', '=', 'users.id')->toSql();
// select users.* from users inner join "posts" on "posts"."user_id" = "users"."id" inner join "comments" on "comments"."post_id" = "posts"."id"
```

But, instead of writting all this, you can just write:

```php
User::select('users.*')->joinRelationship('posts.comments')->toSql();
```

### Has Using Joins

[Querying relationship existence](https://laravel.com/docs/7.x/eloquent-relationships#querying-relationship-existence) is a very powerful and convenient feature of Eloquent. However, it uses the `where exists` syntax which is not always the best and more performant choice, depending on how many records you have or the structure of your table.

This packages implements the same functionality, but instead of using the `where exists` syntax, it uses **joins**.

Below, you can see the methods this package implements and also the Laravel equivalent.

**Laravel Native Methods**

``` php
User::has('posts');
User::has('posts.comments');
User::has('posts', '>', 3);
User::whereHas('posts', function ($query) {
    $query->where('posts.published', true);
});
User::doesntHave('posts');
```

**Package implementations**

```php
User::hasUsingJoins('posts');
User::hasUsingJoins('posts.comments');
User::hasUsingJoins('posts.comments', '>', 3);
User::whereHasUsingJoins('posts', function ($query) {
    $query->where('posts.published', true);
});
User::doesntHaveUsingJoins('posts');
```

### Testing

``` bash
composer test
```

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

### Security

If you discover any security related issues, please email luis@kirschbaumdevelopment.com or nathan@kirschbaumdevelopment.com instead of using the issue tracker.

## Credits

- [Luis Dalmolin](https://github.com/luisdalmolin)

## Sponsorship

Development of this package is sponsored by Kirschbaum Development Group, a developer driven company focused on problem solving, team building, and community. Learn more [about us](https://kirschbaumdevelopment.com) or [join us](https://careers.kirschbaumdevelopment.com)!

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

## Laravel Package Boilerplate

This package was generated using the [Laravel Package Boilerplate](https://laravelpackageboilerplate.com).
