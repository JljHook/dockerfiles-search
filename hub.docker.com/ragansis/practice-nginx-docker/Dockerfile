##Descargamos una versión concreta de UBUNTU, a través del tag
FROM ubuntu:12.04
MAINTAINER Raphael Peraza "raphael.peraza@gmail.com"
##Actualizamos el sistema
RUN apt-get update
##Instalamos nginx
RUN apt-get install -y nginx
##Creamos un fichero index.html en el directorio por defecto de nginx
#RUN echo 'Mi primer Dockerfile' > /usr/share/nginx/www/index.html
ADD web/* /usr/share/nginx/www/
##Arrancamos NGINX a través de ENTRYPOINT para que no pueda ser ##modificar en la creación del contenedor
ENTRYPOINT ["/usr/sbin/nginx", "-g", "daemon off;"]
##Exponemos el Puerto 80
EXPOSE 80
