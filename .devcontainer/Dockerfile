# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.191.1/containers/go/.devcontainer/base.Dockerfile

# [Choice] Go version: 1, 1.16, 1.15
ARG VARIANT="1.16"
FROM mcr.microsoft.com/vscode/devcontainers/go:0-${VARIANT}

# [Choice] Node.js version: none, lts/*, 16, 14, 12, 10
ARG NODE_VERSION="none"
RUN if [ "${NODE_VERSION}" != "none" ]; then su vscode -c "umask 0002 && . /usr/local/share/nvm/nvm.sh && nvm install ${NODE_VERSION} 2>&1"; fi

ARG USER_UID=1000
ARG USER_GID=$USER_UID
USER vscode
ENV HOME /home/vscode

ARG KUBECTX_VERSION="0.9.4"
ARG KUSTOMIZE_VERSION="4.4.0"

# [Optional] Uncomment this section to install additional OS packages.
RUN sudo apt-get update && export DEBIAN_FRONTEND=noninteractive && sudo apt-get upgrade -y
#     && apt-get -y install --no-install-recommends <your-package-list-here>

# Install yq, kubectl, kubectx, kubens, kustomize
RUN go install github.com/mikefarah/yq/v4@latest \
    && curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" \
    && chmod +x kubectl && mv kubectl /go/bin \
    && curl -LO https://github.com/ahmetb/kubectx/releases/download/v${KUBECTX_VERSION}/kubectx_v${KUBECTX_VERSION}_linux_x86_64.tar.gz \
    && curl -LO https://github.com/ahmetb/kubectx/releases/download/v${KUBECTX_VERSION}/kubens_v${KUBECTX_VERSION}_linux_x86_64.tar.gz \
    && tar -zxvf kubectx_v${KUBECTX_VERSION}_linux_x86_64.tar.gz kubectx \
    && tar -zxvf kubens_v${KUBECTX_VERSION}_linux_x86_64.tar.gz kubens \
    && rm -f kube*_v${KUBECTX_VERSION}_linux_x86_64.tar.gz \
    && mv kubectx /go/bin && mv kubens /go/bin \
    && curl -LO https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize/v${KUSTOMIZE_VERSION}/kustomize_v${KUSTOMIZE_VERSION}_linux_amd64.tar.gz \
    && tar -zxvf kustomize_v${KUSTOMIZE_VERSION}_linux_amd64.tar.gz kustomize \
    && rm -f kustomize_v${KUSTOMIZE_VERSION}_linux_amd64.tar.gz && chmod +x kustomize && mv kustomize /go/bin

# Install helm
RUN curl https://baltocdn.com/helm/signing.asc | sudo apt-key add - \
    && sudo apt-get install apt-transport-https --yes \
    && echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list \
    && sudo apt-get update \
    && sudo apt-get -y install --no-install-recommends helm

# [Optional] Uncomment this line to install global node packages.
# RUN su vscode -c "source /usr/local/share/nvm/nvm.sh && npm install -g <your-package-here>" 2>&1

# TODO, ADD: 
# kind installation (does it work inside podman?)
# aliases
# operator-sdk
# (controller-gen)
