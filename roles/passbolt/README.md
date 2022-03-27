# Instalación

Una vez levantado el servicio, ejecutar:

```shell
docker-compose exec passbolt su -m -c "/usr/share/php/passbolt/bin/cake \
                                passbolt register_user \
                                -u porpedirquenoquede@gmail.com \
                                -f tomas \
                                -l turbado \
                                -r admin" -s /bin/sh www-data
```

Con el enlace que nos genera, hacemos login.

# Crear JWT keys

Para crear las llaves, aunque con docker ya existen, primero entramos en el contenedor:

```shell
docker exec -it passbolt_passbolt_1 bash
```

Y después ejecutamos estos comandos:

```shell
mkdir -m=770 /etc/passbolt/jwt
chown www-data:www-data /etc/passbolt/jwt/
su -s /bin/bash -c "/usr/share/php/passbolt/bin/cake passbolt create_jwt_keys" www-data
```

Nos aseguramos de que toda la config está bien lanzando un healthcheck:

```shell
su -s /bin/bash -c "/usr/share/php/passbolt/bin/cake passbolt healthcheck" www-data
```

Y dirá algo así:

```shell
JWT Authentication
[PASS] The JWT Authentication plugin is enabled
[PASS] The /etc/passbolt/jwt/ directory is not writable.
[PASS] A valid JWT key pair was found
```
