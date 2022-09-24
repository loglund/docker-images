FROM ubuntu:22.04

ENV DEBIAN_FRONTEND noninteractive

ENV GOLANG_VERSION 1.19.1
ENV GOLANG_TAR go${GOLANG_VERSION}.linux-amd64.tar.gz
ENV GOLANG_DL_URL https://go.dev/dl/${GOLANG_TAR}
ENV GOLANGCI_LINT_VERSION v1.49.0
ENV GOTESTSUM_VERSION 1.8.2

RUN apt-get update
RUN apt-get install -y \
    wget \
    git \
    build-essential \
    ca-certificates \
    nodejs \
    curl \
    python3-pip \
    python3-dev


RUN wget -P /tmp ${GOLANG_DL_URL}

RUN ls

RUN tar -C /usr/local -xzf /tmp/${GOLANG_TAR}
RUN rm /tmp/${GOLANG_TAR}

ENV GOPATH /go
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH
RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 777 "$GOPATH"

RUN go version

# Install golangci-lint
RUN curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/${GOLANGCI_LINT_VERSION}/install.sh | sh -s -- -b $(go env GOPATH)/bin ${GOLANGCI_LINT_VERSION}
RUN golangci-lint --version

RUN go install -v github.com/stamblerre/gocode@latest
RUN go install -v github.com/ramya-rao-a/go-outline@latest
RUN go install -v golang.org/x/tools/gopls@latest

WORKDIR $GOPATH

ENTRYPOINT ["/bin/bash"]
