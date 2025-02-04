<h1 align="center">PHPLint</h1>

<p align="center">`phplint` is a tool that can speed up linting of php files by running several lint processes at once.</p>

![artboard 1](https://user-images.githubusercontent.com/1472352/38774811-3f780ab6-40a3-11e8-9a0a-a8d06d2c6463.jpg)

[![Release Status](https://github.com/overtrue/phplint/actions/workflows/build-phar.yml/badge.svg)](https://github.com/overtrue/phplint/actions/workflows/build-phar.yml)
[![Latest Stable Version](https://poser.pugx.org/overtrue/phplint/v/stable.svg)](https://packagist.org/packages/overtrue/phplint) [![Total Downloads](https://poser.pugx.org/overtrue/phplint/downloads.svg)](https://packagist.org/packages/overtrue/phplint) [![Latest Unstable Version](https://poser.pugx.org/overtrue/phplint/v/unstable.svg)](https://packagist.org/packages/overtrue/phplint) [![License](https://poser.pugx.org/overtrue/phplint/license.svg)](https://packagist.org/packages/overtrue/phplint)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/overtrue/phplint/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/overtrue/phplint/?branch=master)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fovertrue%2Fphplint.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fovertrue%2Fphplint?ref=badge_shield)


## Installation1

### Locally, if you have PHP

```shell
// PHP 8
$ composer require overtrue/phplint --dev -vvv

// PHP 7
$ composer require overtrue/phplint:^2.0 --dev -vvv
```

### Locally, if you only have Docker

```
// PHP 8
docker pull overtrue/phplint:8.0

// PHP 7
docker pull overtrue/phplint:7.4
```

## Usage

### CLI

```shell
Usage:
  phplint [options] [--] [<path>]...

Arguments:
  path                               Path to file or directory to lint.

Options:
      --exclude=EXCLUDE              Path to file or directory to exclude from linting (multiple values allowed)
      --extensions=EXTENSIONS        Check only files with selected extensions (default: php)
  -j, --jobs=JOBS                    Number of parallel jobs to run (default: 5)
  -c, --configuration=CONFIGURATION  Read configuration from config file (default: ./.phplint.yml).
      --no-configuration             Ignore default configuration file (default: ./.phplint.yml).
      --no-cache                     Ignore cached data.
      --cache=CACHE                  Path to the cache file.
      --json[=JSON]                  Output JSON results to a file.
      --xml[=XML]                    Output JUnit XML results to a file.
  -w, --warning                      Also show warnings
  -h, --help                         Display this help message
  -q, --quiet                        Do not output any message
  -V, --version                      Display this application version
      --ansi                         Force ANSI output
      --no-ansi                      Disable ANSI output
  -n, --no-interaction               Do not ask any interactive question
  -v|vv|vvv, --verbose               Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug
Help:
 Lint something
```

example:

```shell
$ ./vendor/bin/phplint ./ --exclude=vendor
```

You can also define configuration as a file `.phplint.yml`:

```yaml
path: ./
jobs: 10
cache: build/phplint.cache
extensions:
  - php
exclude:
  - vendor
warning: false
```

```shell
$ ./vendor/bin/phplint
```

By default, the command will read configuration from file `.phplint.yml` of path specified, you can custom the filename by option: `--configuration=FILENAME` or `-c FILENAME`;

If you want to disable the config file, you can add option `--no-configuration`.

### Docker cli

```bash
$ docker run overtrue/phplint ./  --exclude=vendor
```

### Program

```php
use Overtrue\PHPLint\Linter;

$path = __DIR__ .'/app';
$exclude = ['vendor'];
$extensions = ['php'];
$warnings = true;

$linter = new Linter($path, $exclude, $extensions, $warnings);

// get errors
$errors = $linter->lint();

//
// [
//    '/path/to/foo.php' => [
//          'error' => "unexpected '$key' (T_VARIABLE)",
//          'line' => 168,
//          'file' => '/path/to/foo.php',
//      ],
//    '/path/to/bar.php' => [
//          'error' => "unexpected 'class' (T_CLASS), expecting ',' or ';'",
//          'line' => 28,
//          'file' => '/path/to/bar.php',
//      ],
// ]
```

### GitHub Actions

```yaml
uses: overtrue/phplint@8.0
with:
  path: .
  options: --exclude=*.log
```
for PHP 7:
```
uses: overtrue/phplint@7.4
```

### Other CI/CD (f.e. Bitbucket Pipelines, GitLab CI)

Run this command using `overtrue/phplint:8.0` Docker image:
```
/root/.composer/vendor/bin/phplint ./ --exclude=vendor
```

### Warnings

Not all linting problems are errors, PHP also has warnings, for example when using a `continue` statement within a
`switch` `case`. By default these errors are not reported, but you can turn this on with the `warning` cli flag, or
by setting the `warning` to true in the configuration.


## PHP 扩展包开发

> 想知道如何从零开始构建 PHP 扩展包？
>
> 请关注我的实战课程，我会在此课程中分享一些扩展开发经验 —— [《PHP 扩展包实战教程 - 从入门到发布》](https://learnku.com/courses/creating-package)

## License

MIT

