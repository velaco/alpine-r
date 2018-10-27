FROM alpine:3.8

MAINTAINER Aleksandar Ratesic "aratesic@gmail.com"

# Declare environment variables ------------------

ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8

ENV BUILD_DEPS \
    cairo-dev \ 
    libxmu-dev \
    openjdk8-jre-base \ 
    pango-dev \
    perl \
    tiff-dev \
    tk-dev

ENV PERSISTENT_DEPS \
    R-mathlib \
    gcc \
    gfortran \
    icu-dev \
    libjpeg-turbo \
    libpng-dev \
    make \
    openblas-dev \
    pcre-dev \
    readline-dev \
    xz-dev \
    zlib-dev \
    bzip2-dev \
    curl-dev

# Install R and R-dev ------------------

RUN apk upgrade --update && \
    apk add --no-cache --virtual .build-deps $BUILD_DEPS && \
    apk add --no-cache --virtual .persistent-deps $PERSISTENT_DEPS && \
    apk add --no-cache R R-dev && \
    apk del .build-deps

CMD ["R", "--no-save"]
