FROM alpine:3.16

RUN set -eux \
    && apk add --update --no-cache \
        bash \
        curl \
        jq

RUN set -eux \
    && cd /tmp \
    && BINARY=yq_linux_amd64 \
    && VERSION="$(curl -fsS -L -o /dev/null -w %{url_effective} https://github.com/mikefarah/yq/releases/latest | sed 's/^.*\///g' )" \
    && curl -fsS -L -O https://github.com/mikefarah/yq/releases/download/${VERSION}/${BINARY}.tar.gz \
    && tar xvfz ${BINARY}.tar.gz \
    && mv ${BINARY} /usr/bin/yq \
    && chmod +x /usr/bin/yq \
    && rm -rf /tmp/* \
    && yq --version | grep -E "$(echo ${VERSION} | cut -c 2-)"

RUN set -eux \
    && KUBECTL_VERSION="$(curl -fsS -L https://storage.googleapis.com/kubernetes-release/release/stable.txt)" \
    && curl -fsS -L -o /usr/bin/kubectl \
        https://storage.googleapis.com/kubernetes-release/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl \
    && chmod +x /usr/bin/kubectl \
    && kubectl version --client --short=true 2>&1 | grep -E "${KUBECTL_VERSION}"

CMD [ "/bin/bash" ]
