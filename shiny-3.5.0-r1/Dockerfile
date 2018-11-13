FROM velaco/alpine-r:base-3.5.0-r1

MAINTAINER Aleksandar Ratesic "aratesic@gmail.com"

# TODO: Separate to build and persistent!
ENV ALL_DEPS \
    automake==1.15.1-r0 \
    autoconf \
    bash \
    m4 \
    file \
    curl-dev \ 
    python2 \ 
    g++ \ 
    gcc \
    cmake \
    libc6-compat \
    git \
    libffi \
    psmisc \
    rrdtool \
    cairo-dev \
    libxt-dev \
    libxml2-dev \
    linux-headers \
    wget

# Installation
RUN echo 'http://dl-cdn.alpinelinux.org/alpine/v3.6/main' >> /etc/apk/repositories && \
    apk upgrade --update && \
    apk add --no-cache $ALL_DEPS && \
    R -e "install.packages(c('shiny'), repos='http://cran.rstudio.com/')" && \
    git clone https://github.com/velaco/shiny-server.git && \
    cd shiny-server && mkdir tmp && cd tmp && \
    DIR=`pwd` && PATH=$DIR/../bin:$PATH && \
    cmake -DCMAKE_INSTALL_PREFIX=/usr/local ../ && \
    wget https://nodejs.org/dist/v8.11.3/node-v8.11.3.tar.xz && \
    mkdir node_source && \
    tar xf node-v8.11.3.tar.xz --strip-components=1 -C "node_source" && \
    rm node-v8.11.3.tar.xz && \
    (cd node_source && ./configure --prefix=/shiny-server/ext/node) && \
    (cd node_source && make) && \
    (cd node_source && make install) && \
    cp /shiny-server/ext/node/bin/node /shiny-server/ext/node/bin/shiny-server && \
    rm /shiny-server/ext/node/bin/npm && \
    (cd ../ext/node/lib/node_modules/npm && ./scripts/relocate.sh) && \
    make && mkdir ../build && \
    (cd .. && ./bin/npm --python="/usr/bin/python" --unsafe-perm install) && \
    (cd .. && ./bin/node ./ext/node/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js --python="/usr/bin/python" rebuild) && \
    make install
    
# Post installation 
RUN ln -s /usr/local/shiny-server/bin/shiny-server /usr/bin/shiny-server && \
    addgroup -g 82 -S shiny; \
    adduser -u 82 -D -S -G shiny shiny && \
    mkdir -p /srv/shiny-server && \
    (cd /srv/shiny-server && chown -R shiny:shiny .) && \
    mkdir -p /var/lib/shiny-server && \
    (cd /var/lib/shiny-server && chown -R shiny:shiny .) && \
    mkdir -p /var/log/shiny-server && \
    (cd /var/log/shiny-server && chown -R shiny:shiny .) && \
    mkdir -p /etc/shiny-server && \
    (cd /etc/shiny-server && chown -R shiny:shiny .)

COPY shiny-server.conf /etc/shiny-server/shiny-server.conf
COPY shiny-server.sh /usr/bin/shiny-server.sh

# Move sample apps to test installation, clean up
RUN chmod 744 /usr/bin/shiny-server.sh && \
    chown shiny:shiny /usr/bin/shiny-server.sh && \
    mv /shiny-server/samples/sample-apps /srv/shiny-server/ && \
    mv /shiny-server/samples/welcome.html /srv/shiny-server/ && \
    rm -rf /shiny-server/ 

EXPOSE 3838

USER shiny

CMD ["/usr/bin/shiny-server.sh"]
