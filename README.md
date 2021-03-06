CoreBundle
==========

everything to quickstart a webpage

[![Build Status]](https://travis-ci.org/c33s/CoreBundle)
[![SemVer]](http://semver.org)
[![Latest Stable Version]](https://packagist.org/packages/c33s/core-bundle) 
[![Latest Unstable Version]](https://packagist.org/packages/c33s/core-bundle) 
[![License]](https://packagist.org/packages/c33s/core-bundle)
[![SensioLabsInsight]](https://insight.sensiolabs.com/projects/c0b45e1c-695f-45d9-ac81-ce2c21ddbb7e)
![Project Status]


Because json is not a really handy format to read and it also lacks in commenting support, this Bundle supports the composer.yml format. [composer-yaml.phar](https://github.com/igorw/composer-yaml) 
is used, to convert from yml to json. In this manual all composer code snippets are in yml format. Create a script file, which call the yml to json converter before running composer. Make sure you 
have both `composer` and `composer-yaml` commands at your fingertips.

## Short Quick Manual

You can perform the whole installation by executing the following commands inside your empty project directory:      

```sh
# Get sample composer file directly from github
wget https://raw.githubusercontent.com/c33s/CoreBundle/master/Resources/files/composer-example.yml -O composer.yml --no-check-certificate
# Modify composer.yml as needed. You may leave this for later.

# Create empty composer.json
touch composer.json

# Convert composer.yml to json format. Do this every time you modify your composer.yml
composer-yaml convert

# Update dependencies without running any scripts. This may take a while.
composer update --no-scripts

# In the following commands, replace "YourNamespace" with your default Namespace prefix you want to use for this project's bundles. Keep it short but helpful.
./bin/init-symfony run YourNamespace

# Now that the project structure is here it's time to run those fancy composer scripts
composer run-script post-update-cmd

# Init basic configuration
php app/console c33s:init-config YourNamespace

# Generate cms structure (webpage and admin bundles)
php app/console c33s:init-cms YourNamespace

# Optional: generate AdminGeneratorGenerator configuration that is automatically patched and correctly integrated into your project
php app/console admin:c33s:build YourNamespace

# This command will clear your cache and pre-render assets
php app/console c33s:clean
```

Make sure the web server permissions are set up correctly. This includes the path for media uploads as well as the sqlite database used by default.
See [http://symfony.com/doc/master/book/installation.html#configuration-and-setup](http://symfony.com/doc/master/book/installation.html#configuration-and-setup) for further information.

You should enable web server writing for the following folders: 

```sh
app/cache
app/logs
app/data
web/media
```

If this goes well, you should see some example pages as well as a secured admin login when accessing /admin/.

## Features

### Propel Model Traits
A helper Trait which can be used to extend your propel model classes, to easily load data from your fixtures for 1:n relations. The data can be directly defined in the fixture file for the object which the data is related to.


By extending your class like this:

ACME/ModelBundle/YourPropelObject.php
```
class YourPropelObject extends BaseYourPropelObject
{
    use \c33s\ModelBundle\Traits\PropelModelTraits;

    public function setYourDataFromArray($data)
    {
	$properties = array
	(
	    'model' => 'ACME\\ModelBundle\\Model\\YourExtraModel',
	);

	return $this->setRelationFromDataArray($data, $properties);
    }
```
you can define fixtures like this:

app/propel/fixtures/yourfixtures.yml
```
ACME\ModelBundle\Model\YourPropelObject:
    YourPropelObject_1:
	 	# normal m:n relation fixtures
        object_has_groups:
            - Group_1
            - Group_2
     	# load 
        your_data_from_array:
            - name: your data name1
              type:  type1
            - value: your data name2
              type:  type2
```


<!-- === references ============================================================================ -->
<!-- badges -->
[Build Status]:            https://img.shields.io/travis/c33s/CoreBundle.svg
[SemVer]:                  https://img.shields.io/:semver-master-orange.svg
[Latest Stable Version]:   https://poser.pugx.org/c33s/core-bundle/v/stable.png
[Latest Unstable Version]: https://poser.pugx.org/c33s/core-bundle/v/unstable.png
[License]:                 https://poser.pugx.org/c33s/core-bundle/license.png
[SensioLabsInsight]:       https://insight.sensiolabs.com/projects/c0b45e1c-695f-45d9-ac81-ce2c21ddbb7e/mini.png
[Project Status]:          https://img.shields.io/maintenance/no/2016.svg
[Packagist Version]:       http://img.shields.io/packagist/v/c33s/core-bundle.svg
[Packagist License]:       http://img.shields.io/packagist/l/c33s/core-bundle.svg

<!-- disabled 
[Build Status disabled1]:            https://travis-ci.org/c33s/CoreBundle.svg?branch=bootstrap3
[Build Status disabled2]:            http://img.shields.io/travis/c33s/CoreBundle/bootstrap3.svg
[SemVer disabled1]:                  http://img.shields.io/:semver-master-brightgreen.svg
 -->

<!-- links -->

<!-- unused -->
[composer]: http://getcomposer.org/
[convention-over-configuration]: http://en.wikipedia.org/wiki/Convention_over_configuration
[coveralls]: https://coveralls.io/
[github pages]: http://pages.github.com/
[github default branch]: https://help.github.com/articles/setting-the-default-branch-for-a-repository
[pathogen]: https://github.com/eloquent/pathogen
[psr-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[sami]: https://github.com/fabpot/Sami
[travis ci]: https://travis-ci.org/
