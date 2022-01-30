FROM bitnami/git as src
ARG GIT_URL
ARG GIT_TAG
# sha1 is mandatory for cache reason (if a tag or branch is moved)
ARG GIT_SHA1
LABEL version=$GIT_TAG
RUN git clone --branch "$GIT_TAG" --depth 1 "$GIT_URL" /app

FROM python:3.9-alpine as final
EXPOSE 80
RUN apk update && apk upgrade
ARG GIT_TAG
LABEL version=$GIT_TAG
COPY --from=src /app /app
RUN mkdir -p /var/run && \
    pip install -r /app/requirements.txt && \
    find /app -type f -not -name 'healthchecker' -delete
CMD ["/app/healthchecker"]
