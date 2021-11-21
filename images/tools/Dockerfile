# Copyright IBM Corp. All Rights Reserved.
#
# SPDX-License-Identifier: Apache-2.0

ARG GO_VER
ARG ALPINE_VER
FROM golang:${GO_VER}-alpine${ALPINE_VER} as golang

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories&&apk update
COPY gcc-10.3.1_git20210424-r2.apk /root
RUN apk add --allow-untrusted /root/gcc-10.3.1_git20210424-r2.apk&&rm -rf /root/gcc-10.3.1_git20210424-r2.apk&&apk add  --allow-untrusted --no-cache \
	bash \
	gcc \
	git \
	make \
	musl-dev binutils binutils-gold;

ADD . $GOPATH/src/github.com/hyperledger/fabric
WORKDIR $GOPATH/src/github.com/hyperledger/fabric

FROM golang as tools
ARG GO_TAGS
RUN make configtxgen configtxlator cryptogen peer discover osnadmin idemixgen GO_TAGS=${GO_TAGS}

FROM golang:${GO_VER}-alpine
# git is required to support `go list -m`
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories&&apk update&&apk add --no-cache \
	bash \
	git \
	jq \
	tzdata;
ENV FABRIC_CFG_PATH /etc/hyperledger/fabric
VOLUME /etc/hyperledger/fabric
COPY --from=tools /go/src/github.com/hyperledger/fabric/build/bin /usr/local/bin
COPY --from=tools /go/src/github.com/hyperledger/fabric/sampleconfig ${FABRIC_CFG_PATH}
