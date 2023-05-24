FROM postgres:14-bullseye

RUN sed -i 's/$/ 11/' /etc/apt/sources.list.d/pgdg.list

RUN apt-get update && apt-get install -y --no-install-recommends \
        gnupg \
		postgresql-11=11.20-1.pgdg110+1 \
        postgresql-common \
        apt-transport-https \
        lsb-release \
        wget \
        ca-certificates \
	&& rm -rf /var/lib/apt/lists/*

RUN yes "" | /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh

RUN echo "deb https://packagecloud.io/timescale/timescaledb/debian/ $(lsb_release -c -s) main" | tee /etc/apt/sources.list.d/timescaledb.list

RUN wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | apt-key add -

RUN apt update

RUN apt install -y --no-install-recommends timescaledb-2-postgresql-14

ENV PGBINOLD /usr/lib/postgresql/11/bin
ENV PGBINNEW /usr/lib/postgresql/14/bin

ENV PGDATAOLD /var/lib/postgresql/11/data
ENV PGDATANEW /var/lib/postgresql/14/data

RUN mkdir -p "$PGDATAOLD" "$PGDATANEW" \
	&& chown -R postgres:postgres /var/lib/postgresql

WORKDIR /var/lib/postgresql

COPY docker-upgrade /usr/local/bin/

ENTRYPOINT ["docker-upgrade"]

# recommended: --link
CMD ["pg_upgrade"]
