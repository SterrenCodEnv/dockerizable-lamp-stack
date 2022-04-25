<p align="center">
  <a href="https://reactjs.org">
    <img src="https://www.alumagubi.co.id/wp-content/uploads/2018/07/lamp_stack.jpg" alt="Logo" width=480 height=auto>
  </a>

<h3 align="center">Dockerizable LAMP</h3>


## Descripción

Este repositorio contiene la estructura basica para crear un servidor LAMP local con Docker Compose.


## Estructura del repositorio

```text
.
├── docker-compose.yaml
├── php.Dockerfile
└── www
    └── index.php
```

### docker-compose.yaml

```yaml
version: "3.7"
services:
  web-server:
    build:
      dockerfile: php.Dockerfile
      context: .
    restart: always
    volumes:
      - "./www/:/var/www/"
    ports:
      - "80:80"
  mysql-server:
    image: mysql:8.0.19
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: secret
    volumes:
      - mysql-data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin:5.0.1
    restart: always
    environment:
      PMA_HOST: mysql-server
      PMA_USER: root
      PMA_PASSWORD: secret
    ports:
      - "8000:80"
volumes:
  mysql-data:
```

### php.Dockerfile

```dockerfile
FROM php:7.4.3-apache
RUN docker-php-ext-install mysqli pdo pdo_mysql
```


## Descarga, instalación e inicialización

Descargar el repositorio con el método a elección haciendo click en el botón "Code▼".

### Instalaciones previas

<div>
<a target:"_blank" href="https://docs.docker.com/engine/install/"><b>Docker</b></a>
</div>

<div>
<a target:"_blank" href="https://docs.docker.com/compose/install/"><b>Docker Compose</b></a>
</div>


### Iniciar servidor LAMP

Una vez instalado Docker y Docker Compose.

1. Corremos el siguiente comando en el directorio donde se descargo el repositorio.
   
   ```docker
   docker-compose up -d
   ```

2. Para corroborar el estado de los servicios ejecuta
   
   ```docker
   docker-compose ps
   ```

3. Por defecto los servicios se ejecuta como se detalla a continuacion 
   
   | Nombre            | Puerto |
   | ----------------- | ------ |
   | MySQL Server      | 3306   |
   | phpMyAdmin        | 8000   |
   | Web Server APACHE | 80     |

4. Para ingresar a phpMyAdmin
   
   ```bash
   localhost/8000
   ```
   
   | Propiedad             | Valor        |
   | --------------------- | ------------ |
   | Usuario Administrador | root         |
   | Clave                 | secret       |
   | Host Name             | mysql-server |

5. Se recomienda crear una base de datos con el nombre "app1" para validar la conexion

6. Si todo funciona correctamente al dirigirse a localhost desde el navegador deberia aparecer el mensaje "Connected successfull" caso contrario "Connection failed" y el error detallado.


### Finalizar Servidor LAMP

Para finalizar la ejecucion del servidor corre el siguiente comando.

```docker
docker-compose down
```


### Eliminar Volumen

Para eliminar el volumen y los servicios asociados

```docker
docker volume rm lamp_mysql-data
```


## Contacto

- Juan Ignacio Sterren [LinkedIn Profile](https://www.linkedin.com/in/sterrenjuan/) - sterrenjuanignacio@gmail.com
