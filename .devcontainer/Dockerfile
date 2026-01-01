ARG PYTHON_VERSION=3.14

FROM mcr.microsoft.com/devcontainers/python:dev-${PYTHON_VERSION}-bookworm

ARG GCLOUD_VERSION=550.0.0
ARG GCLOUD_MD5=e3bc1b41229fa6f9c51cfea30357069c
ARG GCLOUD_FILE=google-cloud-cli-${GCLOUD_VERSION}-linux-x86_64.tar.gz

WORKDIR /tmp

RUN curl -O https://storage.googleapis.com/cloud-sdk-release/${GCLOUD_FILE} \
  && echo "${GCLOUD_MD5} ${GCLOUD_FILE}" | md5sum -c - \
  && tar -xf ${GCLOUD_FILE} -C /usr/local/ \
  && /usr/local/google-cloud-sdk/install.sh --quiet --usage-reporting=false \
  && rm -rf ${GCLOUD_FILE}

ENV PATH="/usr/local/google-cloud-sdk/bin:${PATH}"