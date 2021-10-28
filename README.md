# How to run application

```bash
# run docker composer services
docker-compose up -d
# exec php container
docker-compose exec php bash
# install dependencies inside `php` container
composer install
# run console inside `php` container 
bin/console
```

# How to bind ports

```bash
cp docker-compose.override.example.yml docker-compose.override.yml
```

RabbitMQ Management UI (user: guest, password: guest): http://localhost:15672/

# How to change user ID in php docker container

There are a few options

- add ENVs in `.env`
```bash
DOCKER_COMPOSE__USER_ID=1000
DOCKER_COMPOSE__GROUP_ID=1000
```
- export ENVs
```bash
export DOCKER_COMPOSE__USER_ID=1000
export DOCKER_COMPOSE__GROUP_ID=1000
```

You need to rebuild docker images to apply new user ID
```bash
# Stop containers
docker-compose down
# Build docker images
docker-compose build
```

# Example how application works

```bash
Example #1
student@hillel-php-advanced-2021[docker][/app]: bin/console hillel:task:publish 'Hello, RabbitMQ World!' -vvv
2021-10-16 15:27:02 INFO      [app] Publishing message ...  ["message_body_raw" => "Hello, RabbitMQ World!"]
2021-10-16 15:27:02 INFO      [app] Message was published  ["message_body_raw" => "Hello, RabbitMQ World!"]

student@hillel-php-advanced-2021[docker][/app]: bin/console rabbitmq:consumer task -vvv
2021-10-16 15:27:06 INFO      [app] Received message  ["message_body_raw" => "Hello, RabbitMQ World!"]

Example #2
student@hillel-php-advanced-2021[docker][/app]: bin/console hillel:message-bus:populate -vvv
2021-10-28 21:00:31 INFO      [app] Publishing messages ...
2021-10-28 21:00:31 INFO      [app] Messages were published

student@hillel-php-advanced-2021[docker][/app]: bin/console rabbitmq:consumer logger -vvv
2021-10-28 21:03:29 DEBUG     [app] Received message  ["message_body_raw" => "{"message":"New product was added","context":{"product_id":622}}","routing
_key" => "log.debug"]
2021-10-28 21:03:29 INFO      [app] Received message  ["message_body_raw" => "{"message":"Product was updated","context":{"product_id":771}}","routing_k
ey" => "log.info"]
2021-10-28 21:03:29 ERROR     [app] Received message  ["message_body_raw" => "{"message":"Can not delete brand","context":{"brand_id":584}}","routing_ke
y" => "log.error"]

```

# Tips

- Setup exchanges/bindings/queues in RabbitMQ
```bash
bin/console rabbitmq:setup-fabric
```

# PHPStorm plugins

- https://plugins.jetbrains.com/plugin/7219-symfony-support

# Useful links

- https://getcomposer.org/
- https://www.php.net/supported-versions
- https://symfony.com/doc/current/index.html
- https://www.enterpriseintegrationpatterns.com/patterns/messaging/
- https://hub.docker.com/_/php
- https://hub.docker.com/_/rabbitmq
- https://www.rabbitmq.com/
- https://www.rabbitmq.com/tutorials/tutorial-one-php.html
- https://www.rabbitmq.com/tutorials/tutorial-two-php.html
- https://www.rabbitmq.com/tutorials/tutorial-three-php.html
- https://www.rabbitmq.com/tutorials/tutorial-five-php.html
- https://www.rabbitmq.com/tutorials/tutorial-six-php.html
- https://www.rabbitmq.com/tutorials/tutorial-seven-php.html
