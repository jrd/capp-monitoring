FROM grafana/grafana:8.3.4
USER root
COPY --chown=root ./entrypoint /
RUN apk update && apk upgrade
ENTRYPOINT ["/entrypoint"]
USER grafana
