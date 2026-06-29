FROM golang:1.25.5 AS build-env

WORKDIR /src/github.com/AliyunContainerService/kube-eventer

ENV GO111MODULE=on
ENV CGO_ENABLED=0
ENV GOPROXY=https://goproxy.cn,direct

COPY go.mod go.sum ./
RUN go mod download

COPY . .

ARG VERSION=v1.2.0
ARG GIT_COMMIT=unknown
RUN make VERSION="${VERSION}" GIT_COMMIT="${GIT_COMMIT}"


FROM alpine:3.11.6

RUN apk add ca-certificates tzdata --no-cache
ENV TZ=UTC
RUN addgroup -g 1000 nonroot && \
    adduser -u 1000 -D -H -G nonroot nonroot
USER nonroot:nonroot
ENTRYPOINT ["/kube-eventer"]

COPY --from=build-env /src/github.com/AliyunContainerService/kube-eventer/kube-eventer /
