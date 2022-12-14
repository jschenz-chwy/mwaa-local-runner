# VERSION 1.10
# AUTHOR: Subash Canapathy
# DESCRIPTION: Amazon MWAA Local Dev Environment
# BUILD: docker build --rm -t amazon/mwaa-local .

FROM amazonlinux
LABEL maintainer="amazon"

# Airflow
## Version specific ARGs
ARG AIRFLOW_VERSION=2.4.2
ARG WATCHTOWER_VERSION=3.0.0
ARG PROVIDER_AMAZON_VERSION=6.0.0

## General ARGs
ARG AIRFLOW_USER_HOME=/usr/local/airflow
ARG AIRFLOW_DEPS=""
ARG PYTHON_DEPS=""
ARG SYSTEM_DEPS=""
ARG INDEX_URL=""
ARG PYTHON_VERSION="${PYTHON_VERSION}"
ENV PYTHON_VERSION="${PYTHON_VERSION}"
ENV AIRFLOW_HOME=${AIRFLOW_USER_HOME}

COPY script/bootstrap.sh /bootstrap.sh
COPY script/systemlibs.sh /systemlibs.sh
COPY script/generate_key.sh /generate_key.sh
COPY config/constraints.txt /constraints.txt
COPY config/requirements.txt /requirements.txt
COPY config/mwaa-base-providers-requirements.txt /mwaa-base-providers-requirements.txt

RUN chmod u+x /systemlibs.sh && /systemlibs.sh
RUN chmod u+x /bootstrap.sh && /bootstrap.sh
RUN chmod u+x /generate_key.sh && /generate_key.sh

# Post bootstrap to avoid expensive docker rebuilds
COPY script/entrypoint.sh /entrypoint.sh
COPY config/airflow.cfg ${AIRFLOW_USER_HOME}/airflow.cfg
COPY config/webserver_config.py ${AIRFLOW_USER_HOME}/webserver_config.py

RUN chown -R airflow: ${AIRFLOW_USER_HOME}
RUN chmod +x /entrypoint.sh

EXPOSE 8080 5555 8793

USER airflow
WORKDIR ${AIRFLOW_USER_HOME}
ENTRYPOINT ["/entrypoint.sh"]
CMD ["local-runner"]
