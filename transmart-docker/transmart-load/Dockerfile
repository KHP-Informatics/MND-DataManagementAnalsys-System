FROM debian:jessie
MAINTAINER Denny Verbeeck <dverbeec@its.jnj.com>

RUN apt-get update && apt-get install -y --no-install-recommends curl unzip make ca-certificates
# This is the latest commit at February 1st 2017
RUN curl -L https://github.com/transmart/transmart-data/archive/7c492340e43eaf4366c6bac003a273a4287f3cad.zip -o transmart-data.zip && \
    unzip transmart-data.zip && \
    mv transmart-data-7c492340e43eaf4366c6bac003a273a4287f3cad transmart-data && \
    rm transmart-data.zip

WORKDIR /transmart-data

ADD kettle.properties /root/.kettle/kettle.properties

RUN apt-get install -y --no-install-recommends postgresql-client openjdk-7-jdk php5-cli php5-json xz-utils zip

# install groovy
RUN curl -s "https://get.sdkman.io" | bash && \
    bash -c "source /root/.sdkman/bin/sdkman-init.sh && \
    sdk install groovy 2.4.5"

ENV TABLESPACES=dummy \
    PATH=/root/.sdkman/candidates/groovy/2.4.5/bin:$PATH \
    KETTLE_JOBS_PSQL=/transmart-data/env/tranSMART-ETL/Postgres/GPL-1.0/Kettle/Kettle-ETL/ \
    KITCHEN=/transmart-data/env/data-integration/kitchen.sh \
    PGHOST=tmdb \
    PGPORT=5432 \
    PGDATABASE=transmart \
    PGUSER=docker \
    PGPASSWORD=docker \
    PGSQL_BIN=/usr/bin

RUN make -C env data-integration && \
    make update_datasets

# This is the latest commit at November 9th 2016, the week after the release of eTRIKS v3.0
RUN cd env && curl -L https://github.com/transmart/tranSMART-ETL/archive/01fff8f9469349b5e2673a0a6113bb75d60416a9.zip -o tranSMART-ETL.zip && \
    unzip tranSMART-ETL.zip && \
    mv tranSMART-ETL-01fff8f9469349b5e2673a0a6113bb75d60416a9 tranSMART-ETL && \
    rm tranSMART-ETL.zip

CMD ["echo","Use the make commands provided by samples/postgres to load data. E.g. run\ndocker-compose run --rm tmload make -C samples/postgres load_clinical_ElevadaGSE14468\nFor more information go to https://wiki.transmartfoundation.org/display/transmartwiki/Curated+Data."]
