# hadolint global ignore=DL3006,DL3018

ARG BUILD_IMAGE=alpine:3.18

FROM ${BUILD_IMAGE}

ARG CURL="curl -fsSL"

SHELL [ "/bin/ash", "-euxo", "pipefail", "-c" ]
WORKDIR /usr/local/bin

RUN apk add --update --no-cache \
        bash \
        curl \
        jq

RUN YQ_VERSION="$(${CURL} -o /dev/null -w %\{url_effective\} https://github.com/mikefarah/yq/releases/latest | sed 's/^.*\///g' )" \
    && ${CURL} "https://github.com/mikefarah/yq/releases/download/${YQ_VERSION}/yq_linux_amd64.tar.gz" | tar xzO ./yq_linux_amd64 > yq \
    && chmod +x yq \
    && yq --version | grep -E "${YQ_VERSION}"

RUN KUBECTL_VERSION="$(${CURL} https://storage.googleapis.com/kubernetes-release/release/stable.txt)" \
    && ${CURL} -o kubectl "https://storage.googleapis.com/kubernetes-release/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl" \
    && chmod +x kubectl \
    && kubectl version --client 2>&1 | grep -E "${KUBECTL_VERSION}"

WORKDIR /

CMD [ "/bin/bash" ]
