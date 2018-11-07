# Although this Dockerfile will produce an image with
# Shiny Server installed, it won't start the server

FROM velaco/alpine-r:base-3.5.0-r1

MAINTAINER Aleksandar Ratesic "aratesic@gmail.com"

# TODO: Separate to build and persistent!
# curl-dev, python2, g++ and gcc 
# already installed as persistent deps in
# r-base container velaco/alpine-r:base-3.5.0-r1
ENV ALL_DEPS \
    automake==1.15.1-r0 \
    autoconf \
    bash \
    m4 \
    file \
    cmake \
    libc6-compat \
    git \
    libffi \
    psmisc \
    rrdtool \
    cairo-dev \
    libxt-dev \
    libxml2-dev \
    npm \
    nodejs

# Installation
RUN echo 'http://dl-cdn.alpinelinux.org/alpine/v3.6/main' >> /etc/apk/repositories && \
    apk upgrade --update && \
    apk add --no-cache $ALL_DEPS && \
    R -e "install.packages(c('shiny'), repos='http://cran.rstudio.com/')" && \
    git clone https://github.com/velaco/shiny-server.git && \
    cd shiny-server && mkdir tmp && cd tmp && \
    cmake -DCMAKE_INSTALL_PREFIX=/usr/local ../ && \
    make && mkdir ../build && \
    (cd .. && npm --python="/usr/bin/python" --unsafe-perm install) && \
    (cd .. && node /usr/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js --python="/usr/bin/python" rebuild) && \
    make install
    
# Post installation 
RUN ln -s /usr/local/shiny-server/bin/shiny-server /usr/bin/shiny-server && \
    addgroup -g 82 -S shiny; \
    adduser -u 82 -D -S -G shiny shiny && \
    mkdir -p /srv/shiny-server && \
    mkdir -p /var/lib/shiny-server && \
    mkdir -p /etc/shiny-server

COPY shiny-server.conf /etc/shiny-server/shiny-server.conf
COPY shiny-server.sh /usr/bin/shiny-server.sh

# Move sample apps to test installation, clean up
RUN chmod 744 /usr/bin/shiny-server.sh && \
    mv /shiny-server/samples/sample-apps /srv/shiny-server/ && \
    mv /shiny-server/samples/welcome.html /srv/shiny-server/ && \
    rm -rf /shiny-server/
    
EXPOSE 3838

CMD ["/usr/bin/shiny-server.sh"]
