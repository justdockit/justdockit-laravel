![forthebadge](http://forthebadge.com/images/badges/built-by-developers.svg)
# Just DockIt for Laravel
A simple docker development environment built for Laravel.


Set up for CI and Production environments coming soon :)

## Requirements
- [Docker](https://docs.docker.com/engine/installation/) & [Docker compose](https://docs.docker.com/compose/install/)

## What's in the box?
- PHP 7.1
- NGINX 1.11
- MariaDB 10.1
- Node 7
- Redis 3.2
- Beanstalkd & Beanstalkd console.

## New project set up
1) Create an empty project folder.
```bash
mkdir MyAwesomeProject && cd MyAwesomeProject
```

2) Clone Just Dockit into your empty project folder.
```bash
git clone https://github.com/justdockit/justdockit-laravel.git dockit && cd dockit
```

3) Install Laravel in your project folder. 
```bash
./dockit install-laravel MyApp 
# same as running composer create-project --prefer-dist laravel/laravel MyApp
```
If you have composer installed on your machine and you would like to keep the cache in 
sync with the container add the following volume to the docker-compose.yml file:

dockit/docker-compose.yml
```yaml
php:
    ...
    volumes:
      - ~/.composer/cache:/.composer/cache 
    ...
```

4) Add the MyApp folder to the applications container in docker-compose.yml.  
The path is relative to docker-compose.yml.

dockit/docker-compose.yml
```yaml
applications:
    image: tianon/true
    volumes:
      - ../MyApp/:/var/www/html
```

5) Edit docker environment variables.  
If ports 80 and 3306 are already being used on your machine you probably 
want to change the ports the containers forward to. A .env file is 
provided with the following variables:  

dockit/.env 
```
APP_PORT=80
DB_PORT=3306
DB_ROOT_PASS=secret
DB_NAME=homestead
DB_USER=homestead
DB_PASS=secret
BEANSTALKD_CONSOLE_PORT=2080
```

6) Build images and start the containers.
```bash
./dockit up -d --build # this is the same as running docker-compose up -d --build
```

7) Update laravels .env file. and set the Schema default string length since we 
using MariaDB 10.1.  

MyApp/.env
```
...
DB_HOST=mariadb
...

CACHE_DRIVER=redis
SESSION_DRIVER=redis
QUEUE_DRIVER=beanstalkd

REDIS_HOST=redis
...
```
and

MyApp/app/Providers/AppServiceProvider.php
```php
use Illuminate\Support\Facades\Schema;

/**
 * Bootstrap any application services.
 *
 * @return void
 */
public function boot()
{
    Schema::defaultStringLength(191);
}
  
```

8) Inside config/database change the redis client to phpredis.
```php
 'redis' => [

        'client' => 'phpredis',
        ...
```

If all went well you should see the laravel welcome page at localhost or localhost:8000 if you changed the APP_PORT to 8000.


## Workflow

```bash
#Starting and stopping containers.
./dockit stop # or docker-compose stop
./dockit start # or docker-compose start

# Running composer in a container.
./dockit composer

# Running artisan
./dockit art     # if the php container is running, this is a lot quicker.
./docker artisan # starts up a new php container runs artisan command and then removes it.

# Running tests
./docket t    # if the php container is running, this is a lot quicker.
./dockit test # starts up a new php container to runs tests and then removes it.

```

## Caveat
Once you have everything running, if you happen change the location of your laravel folder. You need to updated the applications volume in the docker-compose.yml file and remove the applications container before running any of the above commands. When required the applications container will be rebuilt with the new volume.

```bash
./dockit rm -f applications # or docker-compose rm -f applications
```

Happy Coding!
