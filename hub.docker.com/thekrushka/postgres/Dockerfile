FROM grahamgilbert/postgres
ENV DB_NAME database
ENV DB_USER admin
ENV DB_PASS password
ADD setupRemoteConnections.sh /docker-entrypoint-initdb.d/setupRemoteConnections.sh
RUN chmod 755 /docker-entrypoint-initdb.d/setupRemoteConnections.sh