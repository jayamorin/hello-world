FROM python:3.12.0a5-alpine3.17 AS builder
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1
RUN apk update && apk add --update gcc libc-dev linux-headers libusb-dev

FROM builder AS dependencies
WORKDIR /srv
COPY requirements.txt .
RUN python -m venv venv
RUN venv/bin/pip install --upgrade pip
RUN venv/bin/pip install -r requirements.txt

FROM python:3.12.0a5-alpine3.17 AS release
ARG COMMIT_HASH
ARG BUILD_DATE
ARG BUILD_TAG

LABEL maintainer="Jay Amorin <jay.amorin@gmail.com>"
LABEL org.label-schema.application="hello-world"
LABEL org.label-schema.vcs-url="https://github.com/jayamorin/hello-world"
LABEL org.label-schema.vcs-ref=${COMMIT_HASH}
LABEL org.label-schema.build-date=${BUILD_DATE}
LABEL org.label-schema.version=${BUILD_TAG}

WORKDIR /srv
COPY app app
COPY --from=dependencies /srv/venv venv
EXPOSE 5000
ENTRYPOINT ["./venv/bin/python", "./app/app.py"]