# 🏗️ Infraestructura Docker — GitOps

Este repositorio contiene la infraestructura declarativa de todos los servicios Docker desplegados en el servidor `linux.lan` (10.10.0.194).
Aquí se almacena únicamente código de infraestructura:

- docker-compose.yml
- Dockerfile (si existe)
- Scripts de despliegue
- Documentación

Los datos reales, configuraciones persistentes, bases de datos y logs NO forman parte del repositorio y permanecen en el servidor.

------------------------------------------------------------
📁 ESTRUCTURA DEL REPOSITORIO
------------------------------------------------------------

docker-core/
    .gitignore
    README.md

    homeassistant/
        docker-compose.yml

    influxdb/
        docker-compose.yml

    mariadb/
        docker-compose.yml

    grafana/
        docker-compose.yml

    jellyfin/
        docker-compose.yml

    net-observer/
        docker-compose.yml

    wifi-dashboard/
        docker-compose.yml

    pihole/
        docker-compose.yml

    nodered/
        docker-compose.yml

    wireguard/
        docker-compose.yml

    esphome/
        docker-compose.yml

    heimdall/
        docker-compose.yml

    npm/
        docker-compose.yml

Cada carpeta contiene solo el docker-compose.yml del servicio correspondiente.

------------------------------------------------------------
🔒 DATOS Y VOLÚMENES (NO INCLUIDOS EN GIT)
------------------------------------------------------------

Los datos reales viven en el servidor, por ejemplo:

/docker/core/homeassistant/config
/docker/core/influxdb/data
/docker/core/mariadb/data
/docker/core/grafana/data
/docker/core/jellyfin/config
/docker/core/jellyfin/cache
/docker/core/net-observer/logs
/docker/core/wifi-dashboard/logs
/srv/media

Estos directorios están excluidos mediante .gitignore para evitar subir datos sensibles o pesados.

------------------------------------------------------------
🚀 DESPLIEGUE CON PORTAINER (GITOPS)
------------------------------------------------------------

Portainer está configurado para desplegar automáticamente cada stack desde este repositorio.

Para añadir un stack en Portainer:

1. Stacks → Add stack
2. Seleccionar "Git repository"
3. Configurar:
   - Repository URL:
     git@github.com:TU_USUARIO/docker-core.git
   - Branch:
     main
   - Compose path:
     servicio/docker-compose.yml
4. Activar:
   - Auto-update
   - Pull latest changes
   - Recreate on change
5. Deploy

Portainer monitoriza el repositorio y actualiza los contenedores cuando detecta cambios.

------------------------------------------------------------
🔄 ACTUALIZAR UN SERVICIO
------------------------------------------------------------

1. Editar el docker-compose.yml del servicio
2. Hacer commit:

git add .
git commit -m "Update servicio X"
git push

3. Portainer detectará el cambio y actualizará el stack automáticamente.

------------------------------------------------------------
🆘 RECUPERACIÓN DEL SISTEMA (DISASTER RECOVERY)
------------------------------------------------------------

En caso de reinstalar el servidor:

1. Clonar el repositorio:

git clone git@github.com:TU_USUARIO/docker-core.git /docker/core

2. Restaurar los volúmenes desde backup o snapshot
3. Instalar Portainer
4. Portainer leerá los stacks desde Git y los desplegará

Esto permite reconstruir toda la infraestructura en minutos.

------------------------------------------------------------
🧪 TESTING LOCAL (OPCIONAL)
------------------------------------------------------------

Para probar un stack sin Portainer:

cd servicio/
docker compose up -d

------------------------------------------------------------
🧹 BUENAS PRÁCTICAS
------------------------------------------------------------

- Nunca subir datos, logs o configuraciones persistentes
- Hacer commits pequeños y descriptivos
- Mantener un stack por carpeta
- Usar .env solo si no contiene secretos
- Hacer snapshots periódicos del servidor

------------------------------------------------------------
📌 ESTADO ACTUAL
------------------------------------------------------------

✔️ Todos los contenedores migrados desde VM0  
✔️ Infraestructura declarativa completa  
✔️ Datos persistentes en el servidor  
✔️ Preparado para GitOps con Portainer  

------------------------------------------------------------
🧑‍💻 AUTOR
------------------------------------------------------------

Infraestructura gestionada por Félix.
