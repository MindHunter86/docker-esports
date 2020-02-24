# -*- coding: utf-8 -*-
# vim: ft=Dockerfile

# deploy Dockerfile - csgoman
ARG ESPORTS_SITE_DIR="/srv/esports"


# deploy Dockerfile - release image
FROM node:10-alpine
LABEL maintainer="vkom <admin@vkom.cc>"

# env for cmd exec:
ARG ESPORTS_SITE_DIR
ENV ESPORTS_SITE_DIR $ESPORTS_SITE_DIR

WORKDIR $ESPORTS_SITE_DIR

ADD mntpage.tgz .

# create site dir:
RUN chmod 2110 . \
	&& chown -R root: .

# fix permissions after ADD for site dir:
RUN apk add --no-cache findutils \
	&& find . -type f -exec chmod 0440 {} \; \
	&& find . -type d -exec chmod 2110 {} \; \
	&& apk del findutils

# fix permissions for anonymous users (only for STATIC projects):
RUN chmod -R a=rX .

# remove developer's garbage:
RUN rm -rfv ./__MACOSX

# ESPORTS_SITE_DIR files listing for debugging:
RUN ls -la .

STOPSIGNAL SIGKILL

# run dummy cmd for public serve:
CMD /bin/rm -rfv /media/esports ; /bin/cp -av . /media/esports ; cat /dev/stdout
