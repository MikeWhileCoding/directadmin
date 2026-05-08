# DirectAdmin API client

[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/mvdgeijn/directadmin/main/LICENSE)

> **This is a fork of [mvdgeijn/directadmin](https://github.com/mvdgeijn/directadmin)**, which itself is a fork of the
> original [omines/directadmin](https://github.com/omines/directadmin). This fork ([MikeWhileCoding/directadmin](https://github.com/MikeWhileCoding/directadmin)) expands the API surface with
> actively maintained documentation and additional method implementations as the DirectAdmin API evolves.

This is a PHP client library to manage DirectAdmin control panel servers, originally developed to automate DirectAdmin
server management where existing implementations were unsupported and incomplete.

API documentation is automatically generated on each push and available at:
https://mvdgeijn.github.io/directadmin/api/

## Installation

The recommended way to install this library is through [Composer](http://getcomposer.org):
```bash
composer require mikewhilecoding/directadmin
```

If you're not familiar with `composer` follow the installation instructions for
[Linux/Unix/Mac](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx) or
[Windows](https://getcomposer.org/doc/00-intro.md#installation-windows), and then read the
[basic usage introduction](https://getcomposer.org/doc/01-basic-usage.md).

## Dependencies

The library uses [Guzzle 7](https://github.com/guzzle/guzzle) as its HTTP communication layer. PHP versions supported
are 7.0, 7.1, 8.0, 8.1 and hhvm.

## Basic usage

To set up the connection use one of the base functions:

```php
use Mvdgeijn\DirectAdmin\DirectAdmin;

$adminContext = DirectAdmin::connectAdmin('http://hostname:2222', 'admin', 'pass');
$resellerContext = DirectAdmin::connectReseller('http://hostname:2222', 'reseller', 'pass');
$userContext = DirectAdmin::connectUser('http://hostname:2222', 'user', 'pass');
```

These functions return an
[`AdminContext`](https://mvdgeijn.github.io/directadmin/api/class-Mvdgeijn.DirectAdmin.Context.AdminContext.html),
[`ResellerContext`](https://mvdgeijn.github.io/directadmin/api/class-Mvdgeijn.DirectAdmin.Context.ResellerContext.html), and
[`UserContext`](https://mvdgeijn.github.io/directadmin/api/class-Mvdgeijn.DirectAdmin.Context.UserContext.html)
respectively exposing the functionality available at the given level. All three extend eachother as DirectAdmin uses a
strict is-a model. To act on behalf of a user you can use impersonation calls:

```php
$resellerContext = $adminContext->impersonateReseller($resellerName);
$userContext = $resellerContext->impersonateUser($userName);
```
Both are essentially the same but mapped to the correct return type. Impersonation is also done implicitly
when managing a user's domains:

```php
$domain = $adminContext->getUser('user')->getDomain('example.tld');
```
This returns, if the domain exists, a [`Domain`](https://mvdgeijn.github.io/directadmin/api/class-Mvdgeijn.DirectAdmin.Objects.Domain.html)
instance in the context of its owning user, allowing you to manage its email accounts et al transparently.

## Contributions

As the DirectAdmin API keeps expanding pull requests are welcomed, as are requests for specific functionality.
Pull requests should in general include proper unit tests for the implemented or corrected functions.

For more information about unit testing see the `README.md` in the tests folder.

## Legal

This software was developed for internal use at [Omines Full Service Internetbureau](https://www.omines.nl/)
in Eindhoven, the Netherlands. It is shared with the general public under the permissive MIT license, without
any guarantee of fitness for any particular purpose. Refer to the included `LICENSE` file for more details.

The project is not in any way affiliated with JBMC Software or its employees.
