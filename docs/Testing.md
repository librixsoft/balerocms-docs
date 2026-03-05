
## Testing

### Run Unit Tests

Create tests in `Tests/Framework` and run:

```bash
docker compose up -d phpunit
docker compose exec phpunit composer install
docker compose exec phpunit composer test
```

If `phpunit` is not running, verify with:

```bash
docker compose ps
```

### Coverage prerequisite (Xdebug)

The `phpunit` service must be built with `Dockerfile.phpunit` (it installs/enables Xdebug).

If coverage is not detected, rebuild the service:

```bash
docker compose down
docker compose build --no-cache phpunit
docker compose up -d phpunit
docker compose exec phpunit php -m | grep -i xdebug
```

### Generate Code Coverage with Docker + PHPUnit

```bash
docker compose exec phpunit composer test
```

Or explicitly with PHP args:

```bash
docker compose run --rm phpunit \
  php -d xdebug.mode=coverage ./vendor/bin/phpunit \
  --configuration phpunit.xml
```

It will create: `build/clover.xml`

### Execute Sonar to View Coverage

First, ensure SonarQube is running:

```bash
docker compose up -d db sonarqube
docker compose logs -f sonarqube
```

Wait until SonarQube is operational at `http://localhost:9000`.

Then run scanner:

```bash
docker run --rm \
  -e SONAR_HOST_URL="http://host.docker.internal:9000" \
  -e SONAR_TOKEN="GENERATED_USER_TOKEN" \
  -v "$(pwd)":/usr/src \
  sonarsource/sonar-scanner-cli
```

Use a **User Token** (recommended for project analysis), not a Global token.
