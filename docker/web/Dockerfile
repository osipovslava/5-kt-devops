FROM python:latest

RUN pip install uwsgi

COPY ./uwsgi.ini /etc/uwsgi/

ENV UWSGI_INI /etc/uwsgi/uwsgi.ini
ENV UWSGI_CHEAPER 2
ENV UWSGI_PROCESSES 16

USER root

ARG INSTALL_CRON=false
RUN if [ ${INSTALL_CRON} = true ]; then \
    apt-get update -y && \
    apt-get -y install cron \
;fi

ARG INSTALL_SUPERVISOR=false
RUN if [ ${INSTALL_SUPERVISOR} = true ]; then \
    apt-get update -y && \
    apt-get -y install supervisor \
;fi

ARG INSTALL_NPM=false
ARG NODE_VERSION
RUN if [ ${INSTALL_NPM} = true ]; then \
    apt-get update -yqq && \
    apt-get install -y gnupg2 && \
    curl -sL https://deb.nodesource.com/setup_${NODE_VERSION}.x | bash - && \
    apt-get install -y nodejs npm && \
    apt-get -y install nodejs \
;fi

ARG INSTALL_GIT=false
RUN if [ ${INSTALL_GIT} = true ]; then \
    apt-get -y install git \
;fi

ARG INSTALL_ZIP=false
RUN if [ ${INSTALL_ZIP} = true ]; then \
    apt-get -y install zip unzip \
;fi

ARG INSTALL_IMAGE_OPTIMIZERS=false
RUN if [ ${INSTALL_IMAGE_OPTIMIZERS} = true ]; then \
    apt-get install -y jpegoptim optipng pngquant gifsicle \
;fi

ARG INSTALL_PG_CLIENT=false
RUN if [ ${INSTALL_PG_CLIENT} = true ]; then \
    mkdir -p /usr/share/man/man1 && \
    mkdir -p /usr/share/man/man7 && \
    apt-get install -y postgresql-client \
;fi

COPY ./requirements.txt /opt/
RUN pip install -r /opt/requirements.txt

ARG WORK_DIR=/var/www
WORKDIR ${WORK_DIR}

RUN usermod -u 1000 www-data

CMD [ "uwsgi" ]
