FROM nginx:1.15.9-alpine

RUN rm /etc/nginx/nginx.conf
COPY nginx.conf /etc/nginx/nginx.conf

COPY spotifyClientFiles/static/ /var/www/site/static/

EXPOSE 80