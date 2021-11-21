# Copyright IBM Corp. All Rights Reserved.
#
# SPDX-License-Identifier: Apache-2.0

ARG GO_VER
ARG ALPINE_VER

FROM alpine:${ALPINE_VER} as peer-base
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories&&apk update&&apk add --no-cache tzdata
# set up nsswitch.conf for Go's "netgo" implementation
# - https://github.com/golang/go/blob/go1.9.1/src/net/conf.go#L194-L275
# - docker run --rm debian:stretch grep '^hosts:' /etc/nsswitch.conf
RUN echo 'hosts: files dns' > /etc/nsswitch.conf

FROM golang:${GO_VER}-alpine${ALPINE_VER} as golang
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories&&apk update
COPY gcc-10.3.1_git20210424-r2.apk /root
RUN apk add --allow-untrusted /root/gcc-10.3.1_git20210424-r2.apk&&rm -rf /root/gcc-10.3.1_git20210424-r2.apk&&apk add  --allow-untrusted --no-cache \
	bash \
	git \
	make \
	musl-dev binutils binutils-gold
ADD . $GOPATH/src/github.com/hyperledger/fabric
WORKDIR $GOPATH/src/github.com/hyperledger/fabric

FROM golang as peer
ARG GO_TAGS
RUN make peer GO_TAGS=${GO_TAGS}

FROM peer-base
ENV FABRIC_CFG_PATH /etc/hyperledger/fabric
VOLUME /etc/hyperledger/fabric
VOLUME /var/hyperledger
COPY --from=peer /go/src/github.com/hyperledger/fabric/build/bin /usr/local/bin
COPY --from=peer /go/src/github.com/hyperledger/fabric/sampleconfig/msp ${FABRIC_CFG_PATH}/msp
COPY --from=peer /go/src/github.com/hyperledger/fabric/sampleconfig/core.yaml ${FABRIC_CFG_PATH}
EXPOSE 7051
CMD ["peer","node","start"]
