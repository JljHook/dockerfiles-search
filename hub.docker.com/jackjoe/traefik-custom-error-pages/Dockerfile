# jekyll
FROM jekyll/builder:latest as jekyll
WORKDIR /tmp
COPY . /tmp

RUN jekyll build

# nginx
FROM nginx:alpine as nginx

COPY --from=jekyll /tmp/nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=jekyll /tmp/_site/ /var/www/public/
