FROM postgres:15-bookworm AS builder

RUN apt-get update \
    && apt-get install -y \
	autoconf \
	ca-certificates \
	clang \
	curl \
	gcc \
	git \
	libcurl4-openssl-dev \
	liblz4-dev \
	libssl-dev \
	libzstd-dev \
	make \
	pkg-config \
	postgresql-server-dev-15

WORKDIR /withpartman

# install all extensions
# RUN git clone https://github.com/citusdata/pg_cron.git && \
# 	cd pg_cron && \
# 	make && make install && \
#     cd .. && rm -rf pg_cron

RUN git clone https://github.com/pgpartman/pg_partman.git && \
    cd pg_partman && \
    make && make install && \
    cd .. && rm -rf pg_partman

# RUN git clone https://github.com/hydradatabase/hydra && \
#     cd hydra/columnar &&  \
#     ./configure && \
#     make && make install && \
# 	cd ../.. && rm -rf hydra

# RUN git clone https://github.com/tembo-io/pg_ivm.git && \
# 	cd pg_ivm && \
# 	make && make install && \
# 	cd .. && rm -rf pg_ivm


FROM postgres:15-bookworm

COPY --from=builder /usr/share/postgresql/15/extension /usr/share/postgresql/15/extension
COPY --from=builder /usr/lib/postgresql/15/lib /usr/lib/postgresql/15/lib

COPY . .
RUN cp -f postgresql.conf /usr/share/postgresql/postgresql.conf.sample

USER postgres

