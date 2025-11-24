# Tools & Reference

## SonarQube

- Start SonarQube:

```bash
docker-compose up -d
```

- Run Sonar Scanner:

```bash
docker run --rm \
  -e SONAR_HOST_URL="http://host.docker.internal:9000" \
  -e SONAR_TOKEN="your_generated_token_here" \
  -v $(pwd):/usr/src \
  sonarsource/sonar-scanner-cli
```

- Generate a Sonar token:
  1. Go to http://localhost:9000
  2. Login as `admin/admin`
  3. Navigate to My Account → Security
  4. Generate and copy your token
  5. Replace it in the command above

## Repository

- GitHub: https://github.com/librixsoft/balerocms-docker
- Bitbucket mirror: https://balerocms@bitbucket.org/librixsoft/balerocms.git

## Additional Notes

- phpMyAdmin: http://localhost:8081 (User: `root`, Password: `root`)
- MySQL storage modes: RAM for ephemeral testing, volume for persistent data.
- LAMP: Only Apache + PHP; MySQL is separate.
