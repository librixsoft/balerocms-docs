# Developers

## Production Mode

In `public/index.php` set the environment to production:

```bash
define('APP_ENV', 'dev'); // change to "prod" if you are uploading to your server
```


## Update/Install Front-End Libs

Update library versions in `package.json` and run:

```bash
npm install
```

## Using Third-Party Libraries in BaleroCMS

BaleroCMS is ready to use Composer for dependencies. Integrating Third-Party Libraries is very easy.

### Example: Integrating an ORM (Doctrine)

Install Doctrine ORM:

```bash
composer require doctrine/orm
```

#### Example Model

```php
<?php
namespace App\Models;

use Doctrine\ORM\Mapping as ORM;
use Doctrine\ORM\EntityManager;

#[ORM\Entity]
#[ORM\Table(name: "data_items")]
class DataItemModel
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: "integer")]
    private int $id;

    #[ORM\Column(type: "string")]
    private string $name;

    #[ORM\Column(type: "string")]
    private string $value;

    private string $hello = "hi";

    private EntityManager $em;

    public function __construct(EntityManager $em)
    {
        $this->em = $em;
    }

    public function getId(): int { return $this->id; }
    public function getName(): string { return $this->name; }
    public function setName(string $name): void { $this->name = $name; }
    public function getValue(): string { return $this->value; }
    public function setValue(string $value): void { $this->value = $value; }

    public function getHello(): string { return $this->hello; }
    public function setHello(string $hello): void { $this->hello = $hello; }

    public function getAllItems(): array
    {
        return $this->em->getRepository(self::class)->findAll();
    }

    public function createItem(string $name, string $value): int
    {
        $item = new self($this->em);
        $item->setName($name);
        $item->setValue($value);

        $this->em->persist($item);
        $this->em->flush();

        return $item->getId();
    }
}
```

#### Example Controller

```php
<?php
namespace App\Controllers;

use App\Models\DataItemModel;
use Framework\Attributes\Inject;
use Framework\Attributes\Controller;
use Framework\Core\View;
use Framework\Http\Get;
use Framework\Http\JsonResponse;
use Doctrine\ORM\EntityManager;

#[Controller('/data')]
class DataItemController
{
    #[Inject]
    private View $view;

    #[Inject]
    private DataItemModel $model;

    #[Get('/list')]
    #[JsonResponse]
    public function listItems(): array
    {
        $items = $this->model->getAllItems();
        return array_map(fn($item) => [
            'id' => $item->getId(),
            'name' => $item->getName(),
            'value' => $item->getValue()
        ], $items);
    }

    #[Get('/create')]
    #[JsonResponse]
    public function createItem(): array
    {
        $id = $this->model->createItem('Sample Item', '123');
        return [
            'status' => 'success',
            'id' => $id
        ];
    }

    #[Get('/view')]
    public function renderView()
    {
        $items = $this->model->getAllItems();
        return $this->view->render("data/list.html", ['items' => $items], useTheme: false);
    }
}
```

## Installing Environment for Local Debugging

### 1. Update Homebrew
```bash
brew update
```

### 2. Install PHP (latest stable version, e.g., 8.4)
```bash
brew install php
```

### 3. Verify PHP installation
```bash
php -v
# You should see something like "PHP 8.4.x (cli)"
```

### 4. Install Xdebug (if not included in PHP by default)
```bash
brew install php-xdebug
```

### 5. Enable Xdebug in PHP
# Check php.ini location
```bash
php --ini
```
# Open the loaded php.ini and add/edit:
```
zend_extension="xdebug.so"
xdebug.mode=debug
xdebug.start_with_request=yes
xdebug.client_host=127.0.0.1
xdebug.client_port=9003
```

### 6. Verify Xdebug is active
```bash
php -i | grep xdebug
# You should see xdebug.mode => debug
```

## Debugging Balero CMS with PhpStorm + Xdebug

This document summarizes the steps to set up and debug your Balero CMS project locally using PHP built-in server and PhpStorm.

### 1. Prepare PHP Environment

Check that PHP and Xdebug are installed locally (Homebrew):

```bash
php -v
php -i | grep xdebug
```

You should see something like:

```
PHP 8.4.x ...
with Xdebug v3.x ...
```

> Note: Ensure that `xdebug.mode => debug` and `xdebug.start_with_request => yes`.

### 2. Start PHP Built-in Server

Your CMS `index.php` is in `public/`, so you must start the server pointing to that folder as the DocumentRoot.

#### Option 1: From the project root

```bash
php -S localhost:8081 -t public
```

#### Option 2: From inside `public/`

```bash
cd public
php -S localhost:8081
```

- Now `http://localhost:8081/` points to `public/index.php`
- All friendly URLs work without `/public` in the URL

### 3. PhpStorm Configuration

1. **PHP Servers**:
    - Host: `localhost`
    - Port: `8081`
    - Path mapping: project root → `/` (DocumentRoot)

2. Enable **Listen for PHP Debug Connections** (phone icon ☎️)

3. Set **breakpoints** in your code.

4. Open the URL in the browser:

```
http://localhost:8081/home
http://localhost:8081/test
```

- PhpStorm will stop execution at the breakpoints.

### 4. Additional Notes

- To stop the built-in server: **Ctrl + C** in the terminal.
- When adding new routes, just open the corresponding URL; you don't need to modify the router.
- Keep `xdebug.log` configured to check connection errors if anything fails.

---

Now you can debug your Balero CMS step by step with PhpStorm and Xdebug.

## Running the Application without Docker

### Using PHP Built-in Server

The application can be run using PHP's built-in web server. Make sure to start the server from the `public/` directory:
```bash
cd public
php -S localhost:8080
```

Then open your browser and navigate to:
```
http://localhost:8080
```

**Note:** You can manually generate the route cache by executing `php bin/cache_routes.php` or execute directly using Docker for this!

## Testing

### Run Unit Tests

Create tests in `Tests/Framework` and run:

```bash
docker compose exec phpunit composer install

docker compose exec phpunit composer test
```

### Generate Code Coverage with Docker + PHPUnit

```bash
docker compose run --rm phpunit \
  php -d xdebug.mode=coverage ./vendor/bin/phpunit \
  --configuration phpunit.xml
```

It will create: build/clover.xml

### Execute Sonar to View Coverage

```bash
docker run --rm \
  -e SONAR_HOST_URL="http://host.docker.internal:9000" \
  -e SONAR_TOKEN="GENERATED_TOKEN" \
  -v $(pwd):/usr/src \
  sonarsource/sonar-scanner-cli
```
