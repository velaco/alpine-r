FROM alpine:3.8

MAINTAINER Aleksandar Ratesic "aratesic@gmail.com"

ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8

ENV R_VERSION 3.5.1
ENV R_SOURCE /usr/local/src/R

ENV BUILD_DEPS \
    cairo-dev \ 
    libxmu-dev \
    openjdk8-jre-base \ 
    pango-dev \
    perl \
    tiff-dev \
    tk-dev \
    wget \
    tar

ENV PERSISTENT_DEPS \
    libintl \
    gcc \
    g++ \
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

RUN apk upgrade --update && \
    apk add --no-cache --virtual .build-deps $BUILD_DEPS && \
    apk add --no-cache --virtual .persistent-deps $PERSISTENT_DEPS && \
    mkdir -p $R_SOURCE && cd $R_SOURCE && \
    wget https://cran.r-project.org/src/base/R-3/R-${R_VERSION}.tar.gz && \
    tar -zxvf R-${R_VERSION}.tar.gz && \
    cd R-${R_VERSION} && \
    ./configure --prefix=/usr/local \
                --without-x && \
    make && make install && \
    cd src/nmath/standalone && make && make install && \
    apk del .build-deps
    
RUN apk add --no-cache 

CMD ["R"]
