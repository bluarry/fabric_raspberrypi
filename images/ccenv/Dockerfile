# Copyright IBM Corp. All Rights Reserved.
#
# SPDX-License-Identifier: Apache-2.0

ARG GO_VER
ARG ALPINE_VER
FROM golang:${GO_VER}-alpine${ALPINE_VER}
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories&&apk update
COPY gcc-10.3.1_git20210424-r2.apk /root

RUN apk add --allow-untrusted /root/gcc-10.3.1_git20210424-r2.apk&&apk add  --allow-untrusted --no-cache \
	g++ \
	git \
	musl-dev&&mkdir -p /chaincode/output /chaincode/input
RUN addgroup -g 500 chaincode && adduser -u 500 -D -h /home/chaincode -G chaincode chaincode
RUN chown -R chaincode:chaincode /chaincode
USER chaincode
