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
   ![Credenciales](file:///home/javier/Im%C3%A1genes/Capturas%20de%20pantalla/Captura%20desde%202025-01-16%2012-51-53.png)
