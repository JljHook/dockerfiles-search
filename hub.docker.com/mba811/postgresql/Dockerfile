FROM mba811/debian:latest
MAINTAINER Tsong khapa <ztd811@yahoo.com>
ENV POSTGRESQL_DB NONE
ENV POSTGRESQL_USER docker
CMD /bin/sh -c apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 7FCC7D46ACCC4CF8 && \
	echo "deb http://apt.postgresql.org/pub/repos/apt/ wheezy-pgdg main" > /etc/apt/sources.list.d/postgresql.list && \
	apt-get update && \
	apt-get install -y postgresql-9.4 && \
	apt-get clean && \
	echo -n > /var/lib/apt/extended_states
CMD /bin/sh -c echo "host all all 0.0.0.0/0 md5" >> /etc/postgresql/9.4/main/pg_hba.conf && \
	sed -i -e "s/^#listen_addresses.*=.*/listen_addresses = '*'/" /etc/postgresql/9.4/main/postgresql.conf && \
	sed -i -e "s/^data_directory.*=.*$/data_directory = '\/data'/" /etc/postgresql/9.4/main/postgresql.conf
ADD config /config
ADD supervisord.conf /etc/supervisor/conf.d/postgresql.conf
EXPOSE 5432
