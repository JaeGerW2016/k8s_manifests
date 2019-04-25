FROM envoyproxy/envoy:latest

COPY envoy.yaml /tmpl/envoy.yaml.tmpl
COPY docker-entrypoint.sh /

RUN chmod 500 /docker-entrypoint.sh

RUN apt-get update && \
    apt-get install gettext -y && \
    rm -rf /var/lib/apt/list/* && \
    rm -rf /var/cache/apk/*

ENTRYPOINT ["/docker-entrypoint.sh"]
