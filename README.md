# Overview

This project provides a Docker based skeleton to create and run Drupal projects.

# Setup

Clone this repo, and then ...

1. Create and customize `.env`

   ```
   cp .env.example .env
   ```

2. Edit `.env` and make sure the values match your environment.

3. Start containers

   ```
   docker-comose up -d
   ```

4. Download drupal using composer. This creates a `drupal` directory.

   ```
   docker-compose exec php composer create-project drupal/recommended-project:^8 drupal
   ```

5. Install drush into your `drupal/vendor` directory.

   ```
   docker-compose exec php composer -d/var/www/drupal require drush/drush
   ```

6. Install site using drush. This creates the appropriate database tables.

   ```
   docker-compose exec php drush -y site:install --account-mail=$(git config user.email)  --site-mail=$(git config user.email) --db-url=mysql://drupal:drupal@db:3306/drupal

   ```

   Make note of the `admin` user's auto-generated password that the above command prints at the end.

7. Get web container IP with (don't forget to replace `name_of_web_container`):

   ```
   docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' name_of_web_container

   ```

8. Visit your site by copy-pasting the IP address printed above into a browser's address bar.

9. Login to your site as `admin` using the password printed in the `site:install` step. Or if you don't have that handy, you can also print login reset link using `docker-compose exec php drush uli`.

# Troubleshooting

- See what containers are running:

```
docker-compose ps
```

- Look at the logs

```
docker-compose logs
```

- Look at `php` logs

```
docker-compose logs php
```

- Tail the `php` logs

```
docker-compose logs -f php
```
