ARG MYSQL_TAG

FROM mariadb:${MYSQL_TAG}

COPY init.sql /docker-entrypoint-initdb.d/
