FROM ubuntu:xenial

LABEL vendor="Claranet GmbH"
LABEL maintainer="Martin Weber <martin.weber@de.clara.net>"
LABEL descritpion="Varnish Fullpage Cache - Version 4.1"

ARG VARNISH_VERSION="4.1.9"

ENV VARNISH_VERSION "${VARNISH_VERSION}"
ENV VARNISH_BACKEND "localhost"
ENV VARNISH_BACKEND_PORT "80"

RUN apt-get update \
 && apt-get install -y python curl pwgen vim \ 
 && apt-get install -y automake autotools-dev libedit-dev libjemalloc-dev libncurses-dev libpcre3-dev libtool pkg-config python-docutils python-sphinx graphviz \
 && curl -o /tmp/varnish.tgz https://codeload.github.com/varnishcache/varnish-cache/tar.gz/varnish-${VARNISH_VERSION} \
 && tar -xzf /tmp/varnish.tgz -C /usr/local/src/ && cd /usr/local/src/varnish-cache-varnish-${VARNISH_VERSION} \
 && ./autogen.sh && ./configure && make && make install \
 && apt-get purge -y automake autotools-dev libedit-dev libjemalloc-dev libncurses-dev libpcre3-dev libtool pkg-config python-docutils python-sphinx graphviz
 
RUN apt install libvarnishapi1 \
 && addgroup varnish \
 && adduser --system --disabled-login --no-create-home --ingroup varnish varnish \
 && rm -rf /var/lib/apt/lists/* \
 && mkdir -p /etc/varnish \
 && chown varnish:varnish /etc/varnish -R 
 
COPY bin/docker-varnish-entrypoint /usr/local/bin/

EXPOSE 80

ENTRYPOINT [ "docker-varnish-entrypoint" ]

CMD [ "-a", ":80", \
      "-f", "/etc/varnish/default.vcl", \
      "-s", "malloc,1024m", \
      "-T", ":6082", \
      "-S", "/etc/varnish/secret" \
    ]
