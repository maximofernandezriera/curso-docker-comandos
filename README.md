# Las instrucciones más comunes son:

* FROM: Sirve para especificar la imagen sobre la que voy a construir la mía. Ejemplo: FROM php:7.4-apache

* LABEL: Sirve para añadir metadatos a la imagen mediante clave=valor. Ejemplo: LABEL company=fbmoll

* COPY: Para copiar ficheros desde mi equipo a la imagen. Esos ficheros deben estar en el mismo contexto (carpeta o repositorio). Su sintaxis es COPY [--chown=<usuario>:<grupo>] src dest. Por ejemplo: COPY --chown=www-data:www-data myapp /var/www/html

* ADD: Es similar a COPY pero tiene funcionalidades adicionales como especificar URLs  y tratar archivos comprimidos.
  
* RUN: Ejecuta una orden creando una nueva capa. Su sintaxis es RUN orden / RUN ["orden","param1","param2"]. Ejemplo: RUN apt update && apt install -y git. En este caso es muy importante que pongamos la opción -y porque en el proceso de construcción no puede haber interacción con el usuario.
  
* WORKDIR: Establece el directorio de trabajo dentro de la imagen que estoy creando para posteriormente usar las órdenes RUN,COPY,ADD,CMD o ENTRYPOINT. Ejemplo: WORKDIR /usr/local/apache/htdocs

* EXPOSE: Nos da información acerca de qué puertos tendrá abiertos el contenedor cuando se cree uno en base a la imagen que estamos creando. Es meramente informativo.  Ejemplo: EXPOSE 80

* USER: Para especificar (por nombre o UID/GID) el usuario de trabajo para todas las órdenes RUN,CMD Y ENTRYPOINT posteriores. Ejemplos: USER jenkins / USER 1001:10001

* ARG: Para definir variables para las cuales los usuarios pueden especificar valores a la hora de hacer el proceso de build mediante el flag --build-arg. Su sintaxis es ARG nombre_variable o ARG nombre_variable=valor_por_defecto. Posteriormente esa variable se puede usar en el resto de la órdenes de la siguiente manera $nombre_variable. Ejemplo: ARG usuario=www-data. NO SE PUEDE USAR EN ENTRYPOINT Y CMD

* ENV: Para establecer variables de entorno dentro del contenedor. Puede ser usado posteriormente en las órdenes RUN añadiendo $ delante de el nombre de la variable de entorno. Ejemplo: ENV WEB_DOCUMENT_ROOT=/var/www/html  NO  SE PUEDE USAR EN ENTRYPOINT Y CMD

* ENTRYPOINT: Para establecer el ejecutable que se lanza siempre  cuando se crea el contenedor  con docker run, salvo que se especifique expresamente algo diferente con el flag --entrypoint. Su síntaxis es la siguiente: ENTRYPOINT <command> / ENTRYPOINT ["executable","param1","param2"]. Ejemplo: ENTRYPOINT ["service","apache2","start"]

* CMD: Para establecer el ejecutable por defecto (salvo que se sobreescriba desde la order docker run) o para especificar parámetros para un ENTRYPOINT. Si tengo varios sólo se ejecuta el último. Su sintaxis es CMD param1 param2 / CMD ["param1","param2"] / CMD["command","param1"]. Ejemplo: CMD [“-c” “/etc/nginx.conf”]  / ENTRYPOINT [“nginx”].
