# -*- coding: utf-8 -*-
# vim: ft=Dockerfile

FROM node:12-alpine AS builder
LABEL maintainer="vkom <admin@vkom.cc>"

WORKDIR /usr/src/esports
ADD .

# remove some default shit:
RUN rm -vrf config/* \

# install all project dependencies and build them:
RUN apk add --no-cache build-base python \
	&& npm install -g yarn \
	&& yarn


FROM node:12-alpine
LABEL maintainer="vkom <admin@vkom.cc>"

ARG PROJECT_SITE_DIR="/srv/esports"
WORKDIR $PROJECT_SITE_DIR

# create project user/group:
RUN addgroup --system w3esports \
	&& adduser --system --disabled-password --home "$PROJECT_SITE_DIR" --shell /sbin/nologin --ingroup w3esports w3esports

# copy builded files from builder & fix dir/file perms:
COPY --chown=root:w3esports --from=builder /usr/src/esports $PROJECT_SITE_DIR
RUN apk add --no-cache findutils \
	&& find . -type f -exec chmod 0440 {} \; \
	&& find . -type d -exec chmod 2150 {} \; \
	&& apk del findutils 

# install debug tools:
RUN apk add --no-cache strace

# install tzdata and set Moscow timezone:
RUN apk add --no-cache tzdata \
	&& ln -s /usr/share/zoneinfo/Europe/Moscow /etc/localtime \
	&& echo 'Europe/Moscow' > /etc/timezone

# expose project port & set container user & run:
EXPOSE 4000
USER w3esports:w3esports
CMD yarn start
