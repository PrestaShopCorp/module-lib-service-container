# PrestaShop Service Container for Modules

This repository includes the service container from Symfony that you can use in your PrestaShop Module.

## Pre-requisites

You should install this library only on a PrestaShop environment and with PHP 5.6.0 minimum.

## Installation

```bash
# PrestaShop 1.7+
composer require prestashop/module-lib-service-container

# PrestaShop 1.6
composer require prestashop/module-lib-service-container
composer require symfony/config:^3.4 symfony/dependency-injection:^3.4 symfony/expression-language:^3.4 symfony/yaml:^3.4

```

When this project is successfully added to your dependencies, you can add the new ServiceContainer to your module and use it.
PrestaShop runs with Symfony components from version 1.7, so dependancies are not required anymore here. I you plan to run your module on PrestaShop, Symfony dependencies must be required separately.

## Usage

To use this library, it's simple :
 - First, declare your new service Container in your root module PHP file (like mymodule.php at your root project folder) :
```
    /**
     * @var ServiceContainer
     */
    private $serviceContainer;
```
- And instantiate it in the constructor with the module name and its local path :
```
$this->serviceContainer = new ServiceContainer($this->name, $this->getLocalPath());
```
- You can add a new function on your root module PHP file, like getService, to retrieve your service name in the new service container :
```
    /**
     * @param string $serviceName
     *
     * @return mixed
     */
    public function getService($serviceName)
    {
        return $this->serviceContainer->getService($serviceName);
    }
```
- Then, you have to declare your service in the services.yml file. You must declare your services in the config/ folder. From Symfony 4, services must be explicitely declared as public to be loaded with the method `getService()`;

We split the services in two folders in the config : /front and /admin folders. So the tree should be like :
```
/mymodule
    /config
        /front
            services.yml
        /admin
            services.yml
        common.yml
```
- Of course, you can include a common file, with common services that are use in front and admin project by an import in the services.yml file :
```
imports:
  - { resource: ../common.yml }
```
Now you can add your services in the services.yml like you were in a Symfony project ;)
