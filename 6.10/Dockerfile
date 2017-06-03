# Dockerfile for Javascript Base Images

FROM nginx:1.12.0-alpine

MAINTAINER Mijail Cisneros <m@spbtv.com>

ENV NODE_VERSION 6.10.3
ENV NPM_CONFIG_LOGLEVEL info
ENV NPM_VERSION 5

RUN addgroup -g 1000 node \
    && adduser -u 1000 -G node -s /bin/sh -D node \
    && apk add --no-cache \
        libstdc++ \
    && apk add --no-cache --virtual .build-deps \
        binutils-gold \
        curl \
        g++ \
        gcc \
        gnupg \
        libgcc \
        linux-headers \
        make \
        python \
    # gpg keys listed at https://github.com/nodejs/node#release-team
    && for key in \
        9554F04D7259F04124DE6B476D5A82AC7E37093B \
        94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
        FD3A5288F042B6850C66B31F09FE44734EB7990E \
        71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
        DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
        B9AE9905FFD7803F25714661B63B535A4C206CA9 \
        C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
        56730D5401028683275BD23C23EFEFE93C4CFFFE \
    ; do \
        gpg --keyserver pgp.mit.edu --recv-keys "$key" || \
        gpg --keyserver keyserver.pgp.com --recv-keys "$key" || \
        gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
    done \
    && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION.tar.gz" \
    && curl -SLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
    && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
    && grep " node-v$NODE_VERSION.tar.gz\$" SHASUMS256.txt | sha256sum -c - \
    && tar -xf node-v$NODE_VERSION.tar.gz \
    && cd /node-v$NODE_VERSION \
    && ./configure \
    && make -j$(getconf _NPROCESSORS_ONLN) \
    && make install \
    && apk del .build-deps \
    && cd .. \
    && if [ -x /usr/local/bin/npm ]; then \
        npm install -g npm@${NPM_VERSION} \
        && find /usr/local/lib/node_modules/npm -name test -o -name .bin -type d | xargs rm -rf; \
       fi \
    && rm -Rf /node-v$NODE_VERSION.tar.gz /node-v$NODE_VERSION \
        /SHASUMS256.txt.asc /SHASUMS256.txt \
        /usr/include /usr/share/man /tmp/* /var/cache/apk/* \
        /root/.npm /root/.node-gyp /root/.gnupg \
        /usr/local/lib/node_modules/npm/man /usr/local/lib/node_modules/npm/doc \
        /usr/local/lib/node_modules/npm/html
    #&& rm -rf /etc/ssl  