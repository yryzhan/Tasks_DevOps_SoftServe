FROM ubuntu:16.04

ENV MOODLE_DB_USER=moodle_devops
ENV MOODLE_DB_PASSWORD=1qaz2wsx
ENV MOODLE_DB_DATABASE=moodle
ENV POSTGRES_USER=postgres

RUN apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8

RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" > /etc/apt/sources.list.d/pgdg.list

RUN apt-get update && apt-get install -y python-software-properties software-properties-common postgresql-9.4 postgresql-client-9.4 postgresql-contrib-9.4

USER postgres

RUN    /etc/init.d/postgresql start &&\
    psql --command "CREATE USER $MOODLE_DB_USER WITH SUPERUSER PASSWORD '$MOODLE_DB_PASSWORD';" &&\
    createdb -O $MOODLE_DB_USER $MOODLE_DB_DATABASE

RUN echo "host all  all    0.0.0.0/0  md5" >> /etc/postgresql/9.4/main/pg_hba.conf

RUN echo "listen_addresses='*'" >> /etc/postgresql/9.4/main/postgresql.conf

EXPOSE 5432

VOLUME  ["/etc/postgresql", "/var/log/postgresql", "/var/lib/postgresql"]

CMD ["/usr/lib/postgresql/9.4/bin/postgres", "-D", "/var/lib/postgresql/9.4/main", "-c", "config_file=/etc/postgresql/9.4/main/postgresql.conf"]

