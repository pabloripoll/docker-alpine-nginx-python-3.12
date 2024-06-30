ARG ALPINE_VERSION=3.20

FROM alpine:${ALPINE_VERSION}

# Setup unique user and group - needs bash
RUN apk add --no-cache bash

ARG UID
ARG GID
ENV UID=${UID}
ENV GID=${GID}

RUN addgroup -g ${GID} --system docker
RUN adduser -G docker --system -D -s /bin/sh -u ${UID} docker

# Create project root
RUN mkdir -p /var/www/htdocs

WORKDIR /var/www/htdocs

# Install main packages and remove default server definition
RUN apk add --no-cache \
  curl \
  wget \
  nginx \
  curl \
  zip \
  bash \
  vim \
  git \
  supervisor \
  py3-pip

RUN set -xe \
    && apk add --no-cache --virtual .build-deps \
        libzip-dev \
        freetype-dev \
        icu-dev \
        libmcrypt-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        libxslt-dev \
        patch \
        openssh-client

# Configure nginx - http
#COPY config/nginx.conf /etc/nginx/nginx.conf

# Configure nginx - default server
#COPY config/conf.d /etc/nginx/conf.d/

# Configure supervisord
#COPY config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Make sure files/folders needed by the processes are accessable when they run under the nobody user
RUN chown -R docker.docker /var/www/htdocs /run /var/lib/nginx /var/log/nginx

# Switch to use a non-root user from here on
USER docker

# Add application
COPY --chown=docker.docker ../ /var/www/htdocs/

# Expose port/s nginx will listen
EXPOSE 80 443 8000

# Let supervisord start nginx & php-fpm
#CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]