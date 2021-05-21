# Fallback for `psr/http-client-implementation` using `php-http-curl`

Provides a metapackage for library needing `psr/http-client-implementation` 
that fallback to a default implementation when user does not meet
the initial requirements.

## Usage

```shell
composer require "Neur0toxine/psr.http-client-implementation.php-http-curl:*"
```

Composer will, by preference:
- use an implementation provided by the user
- otherwise, fallbacks to `php-http/curl-client`

## How does it works

This package contains 2 versions:

1. The highest [1.1](https://github.com/Neur0toxine/psr.http-client-implementation.php-http-curl/blob/1.1/composer.json) needs nothing more than your requirements.

1. The lowest [1.0](https://github.com/Neur0toxine/psr.http-client-implementation.php-http-curl/blob/1.0/composer.json) triggers install of `php-http/curl-client`.

Composer will choose this highest version when user already has a package that satisfy `psr/http-client-implementation`.
Otherwise, composer will choose the lowest version and in that case will 
download the following packages: `php-http/curl-client`.

## What problem does it solve?

You are maintaining a library that need an implementation of `psr/http-client-implementation`,
but you don't want requiring a specific implementation. 

ie. You are maintainer of library that use the following composer.json
```json
{
  "name": "acme/lib",
  "require": {
    "psr/http-client-implementation": "^1.0",
  }
}
```

When end users requires you library with the following code 
```json
{
    "name": "end-user/app",
    "require": {
      "acme/lib": "^1.0"
    }
}
```

They probably ends with such error

```shell
composer up

Your requirements could not be resolved to an installable set of packages.

  Problem 1
    - Installation request for acme/lib ^1.0 -> satisfiable by acme/lib[1.0].
    - acme/lib 1.0 requires psr/http-client-implementation ^1.0 -> no matching package found.
```

You can ask user to install a random package, it works, but the DX is very bad,
and confusing at first.

By using `Neur0toxine/psr.http-client-implementation.php-http-curl`, 
users will be able to require their preferred implementation 
or fallback to your default choice

### Example of user that meet the preferred requirements

```json
{
    "name": "end-user/app",
    "require": {
        "acme/lib": "^1.0",
        "third-party/provide-implementation": "^1.0"
    }
}
```
```shell
composer up
...
Package operations: 2 installs, 0 updates, 0 removals
  - Installing acme/lib (1.0)
  - Installing Neur0toxine/psr.http-client-implementation.php-http-curl (1.1)
```

### Example of user that fallback to your recommendations

```json
{
    "name": "end-user/app",
    "require": {
        "acme/lib": "^1.0"
    }
}
```
```shell
composer up
...
Package operations: 3 installs, 0 updates, 0 removals
  - Installing acme/lib (1.0)
  - Installing Neur0toxine/psr.http-client-implementation.php-http-curl (1.0)
  - Installing php-http/curl-client (1.0)
```

## Contributing

This repository is automatically generated. If you want contributing and 
submitting an issue or a Pull Request, please use 
[composer-fallback/generator](https://github.com/composer-fallback/generator).
