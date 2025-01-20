
# Tarea10SXE.
## Instalar Odoo usando docker  
Instalar la versión 17 de Odoo (Comunity)  

1. Creamos un archivo docker-compose.yml:  
nano docker-compose.yml  

Una vez dentro, copiamos y pegamos:  

services:  
  db:  
    image: postgres:15  
    container_name: postgres_db  
    environment:  
      POSTGRES_USER: putin  
      POSTGRES_PASSWORD: putin  
      POSTGRES_DB: odoo  
    volumes:  
      - ./data/db:/var/lib/postgresql/data  
    ports:  
      - "5432:5432"  

  odoo:  
    image: odoo:17  
    container_name: odoo_17  
    depends_on:  
      - db  
    environment:  
      HOST: db  
      USER: putin  
      PASSWORD: putin  
    volumes:  
      - ./data/odoo:/var/lib/odoo  
    ports:  
      - "8069:8069"  

  pgadmin:  
    image: dpage/pgadmin4:latest  
    container_name: pgadmin  
    environment:  
      PGADMIN_DEFAULT_EMAIL: admin@admin.com  
      PGADMIN_DEFAULT_PASSWORD: putin  
    ports:  
      - "5050:80"  
    volumes:  
      - ./data/pgadmin:/var/lib/pgadmin  

2. Creamos un archivo .env:  
nano odoo.env  

Dentro definimos las variables de entorno:  
POSTGRES_USER=odoo  
POSTGRES_PASSWORD=odoo  
POSTGRES_DB=odoo  
PGADMIN_DEFAULT_EMAIL=admin@admin.com  
PGADMIN_DEFAULT_PASSWORD=admin  


3. Comandos para lanzar los contenedores  

Levantar los servicios:  
docker-compose up -d  

Verificar el estado:  
docker-compose ps  

Parar los servicios:  
docker-compose down  

Eliminar contenedores y volúmenes (limpieza completa):  
docker-compose down -v  


4. Conectar PgAdmin a PostgreSQL  

    Abrir PgAdmin en el navegador: http://localhost:5050.  
    Iniciar sesión con:  
        Correo: admin@admin.com  
        Contraseña: admin  
    Crear una nueva conexión al servidor PostgreSQL:  
        Name: OdooDB  
        Host: postgres_db  
        Port: 5432  
        Username: odoo  
        Password: odoo  

5. Configurar Odoo  

    Acceder a Odoo en: http://localhost:8069.  
    Crear una nueva base de datos desde la interfaz inicial de Odoo:  
        Nombre: odoo_db  
        Correo administrador: (por ejemplo, admin@admin.com).  
        Contraseña administrador: Definir tu contraseña.  
    Configurar la base de datos (ya estará enlazada con el contenedor PostgreSQL).  

6. Acceder a la instalación de Odoo  
  -Nos identificamos:
   ![Credenciales](https://drive.google.com/file/d/1t0Wx06-GYQuRUZQ-UaZeC6YRNiePyf2U/view)




   Instalación de Odoo 17 Community y PgAdmin con Docker Compose

Este repositorio proporciona los archivos necesarios para la instalación de Odoo 17 Community y PgAdmin utilizando Docker Compose. A continuación, se describen las instrucciones paso a paso y los comandos necesarios.
Contenido

    Requisitos previos
    Estructura del proyecto
    Explicación del archivo docker-compose.yml
    Instrucciones de instalación
    Comandos útiles
    Capturas de pantalla
    Preguntas

Requisitos previos

    Docker y Docker Compose instalados en el sistema.
    Conexión a Internet para descargar las imágenes necesarias.

Estructura del proyecto

.
├── docker-compose.yml
└── readme.md

Explicación del archivo docker-compose.yml

El archivo docker-compose.yml define los servicios necesarios para la instalación:

    web: Servicio que ejecuta Odoo 17 Community.
        Imagen utilizada: odoo:17.0
        Reinicio automático con restart: always para asegurar que el contenedor se reinicie si se detiene.
        depends_on: Indica que este servicio depende de la base de datos (db).
        Puertos: Expone el puerto 8069 para acceder a la interfaz de Odoo.
        Variables de entorno:
            HOST: Define el nombre del servicio de base de datos.
            USER y PASSWORD: Credenciales de Odoo.

    db: Servicio de base de datos PostgreSQL.
        Imagen utilizada: postgres:15
        Puertos: Expone el puerto 5432 para acceder a la base de datos.
        Variables de entorno:
            POSTGRES_DB: Nombre de la base de datos.
            POSTGRES_USER y POSTGRES_PASSWORD: Credenciales para la conexión.

    pgadmin: Servicio para la interfaz gráfica de administración de PostgreSQL.
        Imagen utilizada: dpage/pgadmin4
        Reinicio automático con restart: always.
        Puertos: Expone el puerto 8080 (puerto interno 80) para acceso web.
        Variables de entorno:
            PGADMIN_DEFAULT_EMAIL y PGADMIN_DEFAULT_PASSWORD: Credenciales para acceder a PgAdmin.
        depends_on: Indica que este servicio depende del servicio db.

Instrucciones de instalación

Lanza los contenedores:

docker-compose up -d

Verifica que los servicios están corriendo:

docker ps

    Accede a Odoo desde tu navegador:
        URL: http://localhost:8069

    Accede a PgAdmin:
        URL: http://localhost:8080
        Configura la conexión a la base de datos usando las credenciales definidas en docker-compose.yml.

Comandos útiles

Ver logs de un contenedor:

docker logs nombre-del-contenedor

Reiniciar contenedores:

docker-compose restart

Capturas de pantalla
Odoo


PgAdmin

Preguntas
¿Que ocurre si en el ordenador local el puerto 5432 está ocupado?

Nos dara error porque el puerto ya esta siendo usado, tendriamos que detener el proceso que lo esta usando o cambiar el puerto en el compose

    Cambiar puerto para la base de datos en docker-compose.yml:

    db:
      ports:
        - "5433:5432"

    Usa el puerto 5433 para conectarte desde PgAdmin.

¿Y si lo estuviese el 8069?

Nos dara error porque el puerto ya esta siendo usado, tendriamos que detener el proceso que lo esta usando o cambiar el puerto en el compose

    Cambiar puerto para Odoo en docker-compose.yml:

    web:
      ports:
        - "8070:8069"

Accede a Odoo usando http://localhost:8070.

    Clona este repositorio:

    git clone https://github.com/mpineirotroncoso/Tarea_10_docker.git
    cd Tarea_10_docker

    Detener los contenedores:

    docker-compose down
