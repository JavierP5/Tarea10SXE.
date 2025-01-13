# Tarea10SXE.

# Instalar Odoo con 2 métodos  

1. Usando docker
2. Instalación manual

## 1.Usando docker  
Instalar la versión 17 de Odoo (comunity)  

Creamos un archivo docker-compose.yml:  
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
      PGADMIN_DEFAULT_PASSWORD: admin
    ports:
      - "5050:80"
    volumes:
      - ./data/pgadmin:/var/lib/pgadmin

