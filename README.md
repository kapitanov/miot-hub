miot-hub
========

MIOT stands for "My IoT" (so MIOT is "My IoT Hub").

This repo contains configuration for

* [Caddy](https://github.com/mholt/caddy) (configured with container auto-discovery),
* [RabbitMQ](https://hub.docker.com/_/rabbitmq/)
* [NGINX](https://hub.docker.com/_/nginx/) (for home page)
* [miot-time](time/README.md)
* [miot-weather](weather/README.md)

How to run
----------

1. Create docker network named `miot`:

   ```shell
   docker network create miot
   ```

2. Define **host** environment variable name `HOSTNAME` containing your domain name:

   ```shell
   export HOSTNAME=my-awesome-domain-name.com
   ```

3. Clone this repository somewhere.
4. Run docker-compose:

   ```shell
   docker-compose up -d
   ```

Connecting to RabbitMQ
----------------------

Open `https://rabbitmq.my-awesome-domain-name.com` to open RabbitMQ Management UI.

Use address `mqtt://rabbitmq.my-awesome-domain-name.com` to connect to RabbitMQ via MQTT.

Adding extra containers with HTTP
---------------------------------

Run a container with the following environment variables:

```
VIRTUAL_HOST=abc.my-awesome-domain-name.com
VIRTUAL_PORT=8012
```

[`caddy`](https://github.com/mholt/caddy) will auto-discover this container, so every http(s) request to `abc.my-awesome-domain-name.com` will be proxied to `YOUR_CONTAINER:8012`

> **NOTE:** Container *must* be attached to the same network you've created to run MIOT (e.g. `miot`)